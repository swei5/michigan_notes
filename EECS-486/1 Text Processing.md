[[2025-01-10]] #InformationRetrieval 

**Information Retrieval** (IR) is **finding material** (usually documents) of an **unstructured nature** (usually text) that satisfies an **information need** from within **large collections** (usually stored on computers).

These days we often think of Web search, but there are also other types of searches, e.g.:
- Search your own computer
- Search knowledge bases
- Search the library catalogue
- Search the deep Web (e.g., search for a certain car on a rental agency web page)

This class mainly focuses on the **indexing** and **retrieval** of textual documents.
- Concerned firstly with retrieving **relevant documents** to a query
- Concerned secondly with retrieving from **large** sets of documents **eﬃciently**

```ad-example
**Example**: A Typical IR Task

Given:
- A corpus of textual natural-language documents
- A user query in the form of a textual string

Find:
- A ranked set of documents that are relevant to the query

![[Pasted image 20250113114412.png|400]]
```

An over-arching view of the information retrieval system may look something like: 

![[Pasted image 20250113114715.png|450]]

We will focus on **text processing** in this section.

![[Pasted image 20250113122037.png|450]]

### Tokenization
**Tokenization** is segmenting text into tokens. A **token** is a sequence of characters, in a particular document at a particular position. A **type** is the class of all tokens that contain the same character sequence. A **term** is a *normalized* type that is included in the IR dictionary.

For example,
- A text is *"I slept and then I dreamed"*
- Tokens are `I`, `slept`, `and`, `then`, `I`, `dreamed`
- Types are `I`, `slept`, `and`, `then`, `dreamed`
- Terms are `sleep`, `dream` (assuming stop word removal)

A simple approach may be to analyze text into a sequence of discrete tokens (words).
- However, sometimes punctuations (e-mail), numbers (years), and case (Republican, e.g.) can be a meaningful part of a Token
	- Sometimes punctuations change the meaning of texts

The simplest approach is to **IGNORE** **all numbers** and **punctuation** and use only **case-insensitive** unbroken strings of alphabetic characters as tokens.
- More careful approach:
	- Separate `? ! ; : “ ‘ [ ] ( ) < >`
	- Care with `.` 
	- Care with `...`

```ad-note
Other things to be mindful of include (that require special tokenization):
- **Whitespaces** in proper names or collocations
	- E.g. San Francisco => San_Francisco
- **Apostrophes** are ambiguous
	- Possessive constructions: *book's*
	- Contractions: *he's*
	- Quotations: *'let it be'*
- **Hyphenations**
	- E.g. *co-education*
- **Period**
	- Abbreviation: *Mr., Dr.*
	- Acronyms: *U.S.A.*
	- File names: `a.out`
- **Numbers**
- **Unusual Strings**
	- E.g. *C++*
- **HTML**
	- Tags, word sizes, URLs
```

Tokenization is also **language dependent**.
- Need to know the language of the document/query
- **Language Identification**, based on classifiers **trained on short character subsequences** as features, is highly effective

Some languages require word segmentation, like Chinese. A standard baseline segmentation algorithm is **Maximum Matching** (greedy): For a word list of Chinese characters:
1. Start a pointer at the beginning of the string
2. Find the longest word in dictionary that matches the string starting at pointer
3. Move the pointer over the word in string
4. Goto 2

---
### Language Identification
The simplest (and often most effective) approach is to **calculate the likelihood** of each candidate language, based on training data.

Given a sequence of words (or characters) $w_{1}^{n}$ ($n$-grams), for each candidate language, calculate:
- $\mathbb{P}(w_{1}^{n}) \approx \prod_{k=1}^{n} \mathbb{P}(w_{k}|w_{k-1})$, where $w_{0}$ is the first word in the sequence
- $\mathbb{P}(w_{i}|w_{i-1})= \frac{C(w_{i-1}w_{i})}{w_{i-1}}$
	- Where $C(w_{i-1}w_{i})$ and $C(w_{i-1})$ are **simple frequency counts** collected from that language’s training data

Then, we choose the language that **maximizes** the probability.

A potential issue is the possibility of having vocabularies with `count=0` (never occurred in the training data) and leads to overall **zero total probability**. Here, we use **smoothing**: simple techniques to **add a very small quantity** to any count: $$\mathbb{P}(w_{i}|w_{i-1})= \frac{C(w_{i-1}w_{i})+1}{w_{i-1}+V}$$ where $V$ is the vocabulary size, i.e., total number of unique words (or characters) in the training data

```ad-note
We often use special symbols to delineate `<start>` or `<end>` ofstring. Then, intuitively, $C(\text{start}, w_{1})$ is the number of times $w_{1}$ occurs at the start of the sequence in training data.
```

#### Stopwords
It is typical to exclude high-frequency words (e.g., function words: *“a”, “the”, “in”, “to”*; pronouns: *“I”, “he”, “she”, “it”*).
- Stopwords are **language dependent**
- For efficiency, store strings for stopwords in a **hash table** to recognize them in constant time
	- E.g. SMART’s common word list (~ 400), WordNet stopword list

Stopwords are traditionally avoided in IR because they appear so frequent that they might mess up the indexing.

However, the trend is away from **using them**: 
- From large stop lists (200-300), to small stop lists (7-12), to none
- Good **compression techniques** (or cheap hardware) mean the cost for including stop words in a system is very small
- Good query **optimization techniques** mean you pay little at query time for including stop words
- We need them for phrases such as
	- *King of Denmark*
	- *'Let it be'*
	- *Flights to London*

#### Normalization
Token normalization is reducing multiple tokens to the **same canonical term**, such that matches occur despite superficial differences.
- Before: tokens stored and indexed in **a normalized form**
- Now:
	- Create equivalence classes, named after one member of the class
		- E.g. `{USA, U.S.A.}`
	- Maintain relations between unnormalized tokens
		- Can be extended with lists of synonyms (car, automobile)
		- Index unnormalized tokens, a query term is expanded into a disjunction of multiple postings lists
		- Perform expansion during index construction

Common techniques include
- Accents and diacritics
	- E.g. *resumé* and *resume*
		- Most important criterion: How do users like to write their queries for these words? Even in languages that standardly have accents, users often may not type them
		- Often best to normalize to a **de-accented** term
- **Case-Folding**: reduce all letters to lower case
	- Allow user-typed ferrari to match Ferrari in documents
	- However, may lead to unintended matches such as *fed* versus *the Fed*
- **Heuristic**: lowercase only some tokens
	- Words at beginning of sentences
	- All words in a title where most words are capitalized
- **True-casing**: use a **classifier** to decide when to fold
	- Trained on many heuristic features

Other normalizations include
- British/American Spellings
- Formats for dates and times

#### Sub-word Tokenization
Traditional tokenization of web-scale data results in extremely large vocabulary size; by having a smaller unit of word, the chances of finding a match in training increase significantly.
- E.g. *dreaming*, *counting*, *running*, *dream*, *count*, *run* versus *dream*, *count*, *run*, *-ing*

##### Byte-Pair Encoding (BPE)
- Originally developed for text compression
- Used by GPT to reduce vocabulary size

Given a large corpus, and a target vocabulary size, the algorithm learn **merge rules**(Training):
1. Pre-tokenize into words
2. Get all the individual characters in your corpus
3. Compute the frequencies of all pairs (=two consecutive tokens)
4. Merge the most frequent pair into the vocabulary.
	- If needed, break ties randomly or choose the first occurrence
1. Recalculate the frequencies taking new token into account
2. Repeat steps 3 and 4 until desired vocabulary size $V$ is reached

BPE is increasingly the tokenization of choice, especially for training LLMs.