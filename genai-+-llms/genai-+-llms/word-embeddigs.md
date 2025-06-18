# Word Embeddings

Word Embeddings are for interpreting and processing Human language in a format that models and computers can understand.&#x20;

> An embedding is a point in an $$N$$-dimensional space, where $$N$$represents the number of dimensions of the embedding.

Embedding ---> transforms discrete words into a continuous vector (in the form of numbers in a matrix) &#x20;

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption><p><sub>A classic example of the effectiveness of these embeddings is the vector arithmetic that yields meaningful analogies, such as <em><mark style="color:red;">'king' - 'man' + 'woman' ≈ 'queen'</mark></em>. The figure below (</sub><a href="http://veredshwartz.blogspot.sg/"><sub>source</sub></a><sub>) shows distributional vectors represented by an n-dimensional vector, where n is the size of the vocabulary.</sub></p></figcaption></figure>

## Techniques:

1. Count-Based Techniques: TF-IDF,  BM25
2. Co-occurrence Static Embedding Techniques: Word2Vec, GloVe, FastText
3. Contextualized Representation Techniques: BERT, ELMo



### Bag of Words

A bag of words is a simple technique in text representation. It represents text data as a vector of word counts, without considering grammar or order.

#### How to use it?

1. Split the Text into words ( tokens)&#x20;
2. Create a vocabulary list of all unique words in the corpus.
3. For each document:
   * Create a vector with length = vocabulary size
   * Each position corresponds to a word in the vocabulary
   * The value is the **count** of that word in the document

Example:

1. Corpus:&#x20;
   1. Doc1: "I love NLP."      &#x20;
   2. Doc2: "NLP is great."
2. Step 1: Vocabulary : \["I", "love", "NLP", "is", "great"]
3. Step 2: Vector Representations:&#x20;
   1. Doc1 → \[1, 1, 1, 0, 0]
   2. Doc2 → \[0, 0, 1, 1, 1]

Each vector index matches the vocabulary order. For instance:

* `"I love NLP"` → `"I"=1`, `"love"=1`, `"NLP"=1`, others=0
* `"NLP is great"` → `"NLP"=1`, `"is"=1`, `"great"=1`, others=0

**Pros**

* Simple to implement
* Works well for basic models

**Cons**

* Ignores word order and context
* Vocabulary can become large
* Doesn’t handle synonyms or semantics

***

### Term Frequency-Inverse Document Frequency (TF-IDF)

**TF-IDF** is an improvement over Bag of Words that not only counts word occurrences, but also **down-weights common words** and **highlights rare but informative ones**. It balances **how frequent a word is in a document** with **how unique it is across the whole corpus**.

**How TF-IDF Works**

**1. Term Frequency (TF):**\
How often a term appears in a document, normalized by document length.

$$
\text{TF}(t, d) = \frac{\text{Number of times term } t \text{ appears in document } d}{\text{Total number of terms in document } d}
$$

**2. Inverse Document Frequency (IDF):**\
How rare a term is across all documents?

$$
\text{IDF}(t) = \log\left(\frac{\text{Total number of Documents}}{\text{Number of documents containing t }}\right)
$$

**3. TF-IDF Score:**

$$
\text{TF-IDF}(t, d) = \text{TF}(t, d) \times \text{IDF}(t)
$$

***

**📌 Example**

**Corpus:**

```
Doc1: "I love NLP"
Doc2: "NLP is great"
Doc3: "I love machine learning"
```

**Vocabulary:**\
\["I", "love", "NLP", "is", "great", "machine", "learning"]

**Step-by-step for "love" in Doc1:**

* TF("love", Doc1) = 1 / 3
* IDF("love") = log(3 / 2) ≈ 0.176
* TF-IDF("love", Doc1) ≈ 0.333 × 0.176 ≈ 0.059

This down-weights "love" because it appears in multiple documents, unlike rarer words like "machine" or "great".

***

**Pros**

* Weighs important/unique terms higher
* Better for filtering out stopwords and frequent terms
* Works well with sparse data in traditional ML models

**Cons**

* Still ignores word order and semantics
* Can struggle with synonyms and polysemy
* IDF can be skewed with a small corpus
