# Nvidia GPU Basics

### Kernels&#x20;

Kernels are kind of custom functions with an execution configurations which run CUDA GPU's that runs in parallel on GPU threads. it acts a core compute unit for task like Vector multiplication.&#x20;

### Threads&#x20;

These are the smallest independent units for execution that runs a single kernel function. it is the building block of wrap. When a kernel is launched, programmer use a special syntax called execution configuration to describe the prallel excecution configuration.

### Wrap&#x20;

A group of 32 threads that execute the exact same instructions together in lockstep (SIMT)

### GRID&#x20;

The complete collection of all thread blocks launched by a single kernel function across the entire GPU

### **Streaming Multiprocessors (SMs)**

CUDA enabled NVIDIA GPUs consist of several Streaming Multiprocessors, or SMs on a die, with attached DRAM. SMs contain all required resources for the execution of kernel code including many CUDA cores. When a kernel is launched, each block is assigned to a single SM, with potentially many blocks assigned to a single SM. SMs partition blocks into further subdivisions of 32 threads called warps and it is these warps which are given parallel instructions to execute.

When an instruction takes more than one clock cycle to complete (or in CUDA parlance, to expire) the SM can continue to do meaningful work _if it has additional warps that are ready to be issued new instructions._ Because of very large register files on the SMs, there is no time penalty for an SM to change context between issuing instructions to one warp or another. In short, the latency of operations can be hidden by SMs with other meaningful work so long as there is other work to be done.

Therefore, of primary importance to utilizing the full potential of the GPU, and thereby writing performant accelerated applications, it is essential to give SMs the ability to hide latency by providing them with a sufficient number of warps which can be accomplished most simply by executing kernels with sufficiently large grid and block dimensions.



### Grid Stride Loop

When working with Larger data, there are more data elements than the threads in the grid. So in such cases threads cannot work on only one element, So it requires benifits of [memory coalescing](https://homepages.math.uic.edu/~jan/mcs572f16/mcs572notes/lec35.html), Which could allow threads to work in prallel to access memory in contigous chuncks, a scenario which the GPU can leverage to reduce the total number of memory operations.

Grid stride loop is a programming pattern where thread processes multiple data elements by looping through an array, stepping forward by the total number of threads in the entire grid. This design pattern provides flexibility for any dataset size, improves debugging and maximizes hardware efficency.&#x20;

#### The Real-World Scenario: Running Llama 3 Inference

When a user interacts with an LLM, the input sequence length changes constantly. One prompt might be 10 tokens long, while the next prompt might be a 4,000-token document. Inside the Llama 3 architecture, the tensor passing through the activation layer has a shape of:

```latex
\(\text{Tensor\ Size}=\text{Batch\ Size}\times \text{Sequence\ Length}\times \text{Hidden\ Dimension}\)
```

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

<br>

