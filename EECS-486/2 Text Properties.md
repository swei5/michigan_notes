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

By Zipf, a word appearing $f$ times has rank $\frac{AN}{f}$. Several words may appear $f$ times in the document - if we assume this applies to the **last** of these, then the number of words occur exactly $f$ times is $r_{f}-r_{f+1}$