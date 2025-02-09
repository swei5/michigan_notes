[[2025-01-17]] #InformationRetrieval 

### Word Distribution
Key observations include
- A few words are very common
	- E.g. *of*, *the*
- Most words are very rare
	- Half of the words appear only once
- Distribution is **heavy-tailed**, since most of the probability mass is in the tail

**Zipf’s Law** models the distribution of terms in a corpus. We define **rank** ($r$) as the **numerical position** of a word in a list sorted by decreasing frequency ($f$). The Law stated that for some constant $k$ $$fr=k$$
In other words, if the probability of word of rank $r$ is $p_{r}$, and $N$ is the total number of word occurrences in the corpus, $$p_{r}=\frac{f}{N}=\frac{A}{r}$$ for some constant $A$.

By Zipf, a word appearing $f$ times has rank $\frac{AN}{f}$. Several words may appear $f$ times in the document - if we assume this applies to the **last** of these, then the **number of words** occur **exactly** $f$ times is $$I_{f}=r_{f}-r_{f+1}=\frac{AN}{f}- \frac{AN}{f+1}= \frac{AN}{f(f+1)}$$
To predict the occurrence frequencies of any given word, we assume the **highest ranking terms** occur once, and therefore have rank $V=AN$ ($r_{f}=\frac{AN}{f}=AN$), where $V$ is the size of the vocabulary. Its fraction (number of words with one occurrence out of the total vocabulary) is $$I_{f}= \frac{AN}{f(f+1)} = \frac{V}{f(f+1)} \to \frac{I_{f}}{V} = \frac{1}{f(f+1)}$$
That being said, in general, the fraction of words appearing only once is roughly 1/2.

```ad-note
**Zipf’s** explanation was the principle of least eﬀort. It balances between speaker’s desire for a small vocabulary and hearer’s desire for a large one.
- Richer gets richer
- Random typing of letters including a space will generate “words” with a Zipfian distribution
```

By Zipf's Law, stopwords will account for a large fraction of text so eliminating them greatly reduces inverted-index storage costs. However, for most words, gathering sufficient data for meaningful statistical analysis (e.g. for correlation analysis for query expansion) is difficult since they are **extremely rare**.
- Length of web pages has a Zipfian distribution
- Number of hits to a web page has a Zipfian distribution

```ad-example
Assuming Zipf’s Law with a corpus constant $A=0.1$, what is the fewest number of most common words that together account for more than $25\%$ of word occurrences (i.e. the minimum value of m such as at least $25\%$ of word occurrences are one of the $m$ most common words)

Answer: We have that $f=\frac{AN}{r}$, and we are looking for $$\begin{align}
\frac{AN}{1} &+ \frac{AN}{2} + \cdots + \frac{AN}{m} > 0.25 \cdot N\\
&\implies 1 + \frac{1}{2} + \cdots + \frac{1}{m} > \frac{0.25}{A}\\
\end{align}$$ where $A=0.1$. We solve for the equation and get $m=7$.
```

---
### Vocabulary Growth
We would like to know how the size of the overall **vocabulary** (number of unique words) grows with the size of the corpus. This determines how the size of the **inverted index will scale** with the size of the corpus.
- Vocabulary not really upper-bounded due to proper names, typos, etc.

**Heap's Law** states that if $V$ is the size of the vocabulary and the $n$ is the length of the corpus in words, then $$V=KN^{\beta}$$ where $V$ is the vocabulary of the collection (number of unique words), $N$ is the total number of word occurrences, and $K,\beta$ are constants.
- Typical $K$ range: $10-100$
- Typical $\beta$ range: $0.4-0.6$

![[Pasted image 20250208220933.png|400]]

---
### Growth Analysis at Scale
The projected growth rate $g$ is around $10\%$ for the training data for large LLM models.

![[Pasted image 20250208221608.png|400]]

Projections of data usage come from
- Extrapolations of data usage from past trends
- Extrapolations of data usage from **compute availability** estimations

The growth trend of data usage and data usage **DO NOT match**. Data is estimated to run out by 2028.

Potential solution: 
- More data (deep web), but there are concerns.
	- Estimates of the total stock produced by dividing the estimate of the number of tokens by the **share of traffic**
- Synthetic (AI) data
	- Mixed effectiveness
	- Works well for settings where it can be verified (math)

