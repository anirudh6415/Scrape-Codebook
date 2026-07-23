# Nvidia GPU Basics

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption><p><a href="https://developer.nvidia.com/blog/cuda-refresher-cuda-programming-model/"><em>Kernel execution on GPU.</em></a></p></figcaption></figure>

### Kernels&#x20;

Kernels are kind of custom functions that run on the device (GPU), called from the host (CPU). They\
have an execution configuration which tells the GPU how many threads to launch in parallel. It acts as the core compute unit for tasks like vector multiplication, matrix multiply, and activation functions\
in neural networks.

How you write a kernel depends on the framework you are using:

| Framework       | How you mark a  kernel | When you use it                                                                          |
| --------------- | ---------------------- | ---------------------------------------------------------------------------------------- |
| CUDA C/C++      |  _`__global__`_        | Low level GPU programming, writing custom ops from scratch                               |
| Triton (Python) | `@triton.jit`          | High performance GPU kernels in Python, used by PyTorch 2.x internally via torch.compile |
| Numba  (Python) | `@cuda.jit`            | Python native CUDA, useful for learning and prototyping                                  |

### Threads&#x20;

These are the smallest independent units of execution that run a single kernel function. It is the\
building block of a warp. When a kernel is launched, the programmer uses a special syntax called an execution configuration to describe the parallel execution setup.

Each thread gets a unique index which it uses to figure out which piece of data it should work on:

In CUDA C / Numba: `idx = blockIdx.x * blockDim.x + threadIdx.x`

In Triton, it works differently. Triton does not think at the individual thread level. Instead it thinks in blocks of data. Each Triton program handles a chunk: `pid = tl.program_id(axis=0) offsets = pid * BLOCK_SIZE + tl.arange(0, BLOCK_SIZE)`

### Wrap&#x20;

A group of 32 threads that execute the exact same instructions together in lockstep. This model is\
called SIMT (Single Instruction, Multiple Threads).&#x20;

> One important thing to know here is **warp divergence**. If threads inside the same warp hit an if/else branch and go different directions, the GPU cannot execute both paths at the same time. It serializes them, runs the if path first with the else threads masked off, then runs the else path. Both paths cost clock cycles even though each thread only does one of them. This can cut performance in half and comes up in interviews a lot.&#x20;
>
> You don't need to completely avoid `if` statements, but you should structure your code and data to avoid divergence _within the same warp_. Common strategies include:&#x20;
>
> 1. **Data Reordering:** Group similar data together prior to processing so that all threads in a single warp make the same control-flow decisions
> 2. **Branchless Programming:** Replace simple conditional logic with mathematical operations (e.g., using `min` or `max`) to ensure all threads execute the same instructions.&#x20;
> 3. **Loop Optimization:** Minimize loop boundaries where threads will exit at highly staggered times.
>
> Rule of thumb: avoid conditional logic inside kernels where threads in the same warp will disagree on the branch.

### Thread Block

A group of threads that execute together on a single SM. Threads inside the same block can talk to each other through shared memory and can synchronize using `__syncthreads()` in CUDA C, Numba `cuda.syncthreads()`, or `tl.debug_barrier()` in Triton.

A few constraints worth knowing:

* Max 1024 threads per block in CUDA C and Numba
* Block size should always be a multiple of 32 (the warp size), otherwise the last warp is only partially filled and you waste compute slots
* In Triton, BLOCK\_SIZE must be a power of 2 (64, 128, 256, 512) because Triton's compiler requires this for its vectorization passes

### GRID&#x20;

The complete collection of all thread blocks launched by a single kernel function across the entire\
GPU.

<figure><img src="../.gitbook/assets/image (27).png" alt="" width="563"><figcaption><p><a href="https://developer.nvidia.com/blog/cuda-refresher-cuda-programming-model/">Source : CUDA kernels are subdivided into blocks.</a></p></figcaption></figure>

Total threads in the $$\text{grid} = \text{grid}_{\text{Dim}} \times \text{block}_{\text{Dim}}$$.&#x20;

### **Streaming Multiprocessors (SMs)**

CUDA enabled NVIDIA GPUs consist of several Streaming Multiprocessors, or SMs on a die, with attached DRAM. SMs contain all required resources for executing kernel code including many scalar processors (NVIDIA markets these as CUDA cores, they are specifically FP32 ALUs), a large register file, shared memory / L1 cache, and warp schedulers.

When a kernel is launched, each block is assigned to a single SM, with potentially many blocks assigned to a single SM. SMs partition blocks into further subdivisions of 32 threads called warps and it is these warps which are given parallel instructions to execute.

When an instruction takes more than one clock cycle to complete, the SM can continue to do meaningful work if it has additional warps that are ready to be issued new instructions. Because of very large register files on the SMs, there is no time penalty for an SM to change context between issuing instructions to one warp or another. The latency of operations can be hidden by SMs with other meaningful work so long as there is other work to be done.

Therefore, of primary importance to utilizing the full potential of the GPU, it is essential to give\
SMs the ability to hide latency by providing them with a sufficient number of warps, which can be\
accomplished most simply by executing kernels with sufficiently large grid and block dimensions.

For reference, the A100 has 108 SMs; the H200 (Hopper) has 132 (SXM) or 114 (NVL) SMs; and the B300 (Blackwell Ultra) features up to 160 SMs across two dies. All three architectures support up to 64 warps and a maximum of 2,048 threads per SM.

### Grid Stride Loop

When working with Larger data, there are more data elements than the threads in the grid. So in such cases threads cannot work on only one element, So it requires benifits of [memory coalescing](https://homepages.math.uic.edu/~jan/mcs572f16/mcs572notes/lec35.html), Which could allow threads to work in prallel to access memory in contigous chuncks, a scenario which the GPU can leverage to reduce the total number of memory operations.

Grid stride loop is a programming pattern where thread processes multiple data elements by looping through an array, stepping forward by the total number of threads in the entire grid. This design pattern provides flexibility for any dataset size, improves debugging and maximizes hardware efficency.&#x20;

#### The Real-World Scenario: Running Llama 3 Inference

When a user interacts with an LLM, the input sequence length changes constantly. One prompt might be 10 tokens long, while the next prompt might be a 4,000-token document. Inside the Llama 3 architecture, the tensor passing through the activation layer has a shape of:

$$
\text{Tensor\ Size}=\text{Batch\ Size}\times \text{Sequence\ Length}\times \text{Hidden\ Dimension}
$$

Because the **Sequence Length** changes with every single user request, the total number of data elements (N) entering the CUDA kernel is completely dynamic and unpredictable.

Instead of trying to dynamically launch a different number of GPU threads for every single prompt (which causes massive CPU-GPU overhead), PyTorch launches a **fixed, highly optimized grid size** (e.g., 65,536 threads) and uses a grid stride loop to process the arbitrary data.

Here is exactly how the math and code work inside that GPU hardware:

1. The Dynamic InputA user submits a long prompt. The dynamic tensor size (N) evaluates to **1,000,000 elements**.
2. The Fixed Hardware LaunchThe framework launches a fixed grid of threads:
   1. **Block Size (`blockDim.x`):** 512 threads per block
   2. **Grid Size (`gridDim.x`):** 128 blocks
   3. **Total Threads in Grid:** 128 X 512 = 65,536 threads
3. Execution via StrideBecause 65,536 threads cannot process 1,000,000 elements all at once, the threads must loop. The **stride** is exactly equal to the total number of threads in the grid (65,536).
   1. **Iteration 1:** Threads `0` to `65,535` process data indices `0` to `65,535` simultaneously.
   2. **Iteration 2:** Every thread jumps forward by the stride (65,536). Thread `0` now processes index `65,536`, thread `1` processes `65,537`, and so on.
   3. **Iteration 3:** Every thread jumps forward by another (65,536) to process indices starting at `131,072`.
   4. This repeats until all 1,000,000 elements are computed.

### Atomic Operations&#x20;

CUDA, like many general purpose parallel execution frameworks, makes it possible to have race conditions in your code. A race condition in CUDA arises when threads read to or write from a memory location that might be modified by another independent thread. Generally speaking, you need to worry about:

* read-after-write hazards: One thread is reading a memory location at the same time another thread might be writing to it.
* write-after-write hazards: Two threads are writing to the same memory location, and only one write will be visible when the kernel is complete.

A common strategy to avoid both of these hazards is to organize your CUDA kernel algorithm such that each thread has exclusive responsibility for unique subsets of output array elements, and/or to never use the same array for both input and output in a single kernel call. (Iterative algorithms can use a double-buffering strategy if needed, and switch input and output arrays on each iteration.)

However, there are many cases where different threads need to combine results. Consider something very simple, like: "every thread increments a global counter." Implementing this in your kernel requires each thread to:

1. Read the current value of a global counter.
2. Compute `counter + 1`.
3. Write that value back to global memory.

However, there is no guarantee that another thread has not changed the global counter between steps 1 and 3. To resolve this problem, CUDA provides **atomic operations** which will read, modify and update a memory location in one, indivisible step.&#x20;

A real world example example of atomic operations a computer vision model analyzes an image, it needs to count how many pixels have a specific brightness value (from 0 to 255). Because millions of pixels share the same color values, thousands of GPU threads will try to increment the exact same counter at the exact same time.&#x20;

<br>

