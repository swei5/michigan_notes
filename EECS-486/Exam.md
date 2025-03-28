### Question 2

According to theZipf's Law, the following stand:
$$f_{1}=\frac{AN}{1}, f_{2}=\frac{AN}{2}, f_{3}=\frac{AN}{3}, f_{4}= \frac{AN}{4}$$
Since we know $f_{1}+f_{2}+f_{3}+f_{4}=250000$, we have that $$\begin{align}
\frac{25}{12}AN&= 250000 \\
1000000A &= 120000\\
A &= 0.12
\end{align}$$

### Question 3
| TF    | Heat    | Water     | Warm    |
| ----- | ------- | --------- | ------- |
| Doc 1 | 1/1 = 1 | 1/1 = 1   | 0/1 = 0 |
| Doc 2 | 0/2 = 0 | 0/2 = 0   | 2/2 = 1 |
| Doc 3 | 0/2 = 0 | 1/2 = 0.5 | 2/2 = 1 |

| IDF | Heat           | Water            | Warm             |
| --- | -------------- | ---------------- | ---------------- |
|     | Log (3)=0.4771 | Log (3/2)=0.1761 | Log (3/2)=0.1761 |

| TF-IDF | Heat   | Water  | Warm   |
| ------ | ------ | ------ | ------ |
| Doc 1  | 0.4771 | 0.1761 | 0      |
| Doc 2  | 0      | 0      | 0.1761 |
| Doc 3  | 0      | 0.0881 | 0.1761 |

$$\begin{align}
\text{sim}(Doc1,Doc2)&= \frac{[0.4771, 0.1761,0]^{T}[0, 0, 0.1761]}{||Doc1||||Doc2||}\\
&= 0\\
\end{align}$$
### Question 4

- `AND` Bear: 4392
- `AND` Fox: 2540
- Mouse `OR` Cat: 10200+
- `AND NOT DOG`: 38200

Thus, we would process the query in the following order:
1. Fox
2. Bear
3. Mouse `OR` Cat 
4. Dog

### Question 5

1.  synthetic `AND` language gives
	- Doc2: synthetic language processing module feature
2. module `OR` (1)
	- Doc1: bug feature code module design
	- Doc2: synthetic language processing module feature
	- Doc3: module linguistic bug language design

### Question 6

$0$. Because if a word appears in every single document, it does not provide any additional information for distinguishing between documents of different classes because its distribution is the same across all classes.

### Question 7

First, we calculate the entropy at the root. There are 4 positive reviews and 5 negative reviews. Thus, the entropy at the current level is $$- \frac{4}{9} \log_{2} \frac{4}{9}- \frac{5}{9} \log_{2} \frac{5}{9}=0.9911$$
Assume we branch out from the word *Boring*, then we have the following child nodes along with their statistics:
- *Boring*: 1 positive, 2 negative
	- Entropy: $$-\frac{1}{3}\log_{2} \frac{1}{3} - \frac{2}{3}\log_{2} \frac{2}{3}=0.9183$$
- *Not Boring*: 3 positive, 3 negative
	- Entropy: $$-\frac{1}{2}\log_{2} \frac{1}{2} - \frac{1}{2} \log_{2} \frac{1}{2}=1$$

Thus, the information gain is $$\begin{align}
&\text{Entropy}(S)-p(\text{Boring})\text{Entropy}(S_{b})-p(\neg\text{Boring})\text{Entropy}(S_{n})\\
&= 0.9911 - \frac{3}{9}(0.9183) - \frac{6}{9}(1)\\
&= 0.0183
\end{align}$$
### Question 8

There are 2 positive and 2 negative labeled documents, so each has a probability of $0.5$ in the model. We then count word frequencies in both the positive and negative labeled documents. 
- Positive:
	- *the*: 2
	- *food*: 2
	- *was*: 1
	- *great*: 1
	- *i*: 1
	- *like*: 1
	- Total words: 8
- Negative:
	- *the*: 2
	- *food*: 1
	- *was*: 2
	- *not*: 1
	- *good*: 1
	- *at*: 1
	- *all*: 1
	- *service*: 1
	- *really*: 1
	- *bad*: 1
	- Total words: 12

Thus, given a post with content *really like food*, we can calculate its probability for each label.
- Positive: $$\begin{align}
&P(\text{pos})\cdot P(\text{really}|\text{pos})P(\text{like}|\text{pos})P(\text{food}|\text{pos})\\
&= \frac{2}{4} \cdot \frac{1}{8+14} \cdot \frac{1+1}{22} \cdot \frac{2+1}{22}\\
&= 0.0002817
\end{align}$$
- Negative: $$\begin{align}
&P(\text{neg})\cdot P(\text{really}|\text{neg})P(\text{like}|\text{neg})P(\text{food}|\text{neg})\\
&= \frac{2}{4} \cdot \frac{1+1}{12+14} \cdot \frac{1}{26} \cdot \frac{1+1}{26}\\
&= 0.0001138
\end{align}$$
Therefore, we predict "Positive" as its label.

### Question 9 (1)

| Current | Queue   |
| ------- | ------- |
| A       | D, F, C |
| D       | F, C, E |
| F       | C, E    |
| C       | E       |
| E       |         |
BFS: A -> D -> F -> C -> E

| Current | Stack   |
| ------- | ------- |
| A       | D, F, C |
| C       | D, F    |
| F       | D       |
| D       | E       |
| E       |         |
DFS: A -> C -> F -> D -> E

### Question 9 (2)

The probability that we land on $B$ from $A$ is given by $$S(B)= \frac{1-d}{N}+d(0)$$ since $B$ has no incoming nodes. We know that $N=6$ and $S(B)=\frac{1}{60}$. Solving the equation yields $d=0.9$.

### Question 9 (3)

$$S(C_{news})= \frac{1-d}{\text{Bias}_{news}}+d \sum\limits_{j \in In(C)} \frac{1}
{|Out(j)|} S(j)$$

$$S(D_{news})= \frac{1-d}{\text{Bias}_{news}}+d \sum\limits_{j \in In(D)} \frac{1}
{|Out(j)|} S(j)$$

Without doing any calculations, $C$ and $D$ appear to have the same News topic-sensitive PageRank (at least in the first iteration), since they both are categorized as news and has the same incoming node of $A$.

### Question 10 (1)

| Rank | Relevant | Precision |
| ---- | -------- | --------- |
| 1    | TRUE     | 1/1       |
| 2    | FALSE    | N/A       |
| 3    | TRUE     | 2/3       |
Thus, the average precision is $\frac{1}{2} \cdot \left (1 + \frac{2}{3}\right)= \frac{5}{6}$.

### Question 10 (2)
| Rank | Doc | Rel Gain | CG_n | Log_n | DCG_n |
| ---- | --- | -------- | ---- | ----- | ----- |
| 1    | 1   | 2        | 2    | -     | 2     |
| 2    | 2   | 0        | 0    | 1     | 2     |
| 3    | 3   | 1        | 3    | 1.58  | 2.63  |

Ideal ranking:

| Rank | Doc | Rel Gain | CG_n | Log_n | IDCG_n |
| ---- | --- | -------- | ---- | ----- | ------ |
| 1    | 1   | 2        | 2    | -     | 2      |
| 2    | 3   | 1        | 3    | 1     | 3      |
| 3    | 2   | 0        | 0    | 1.58  | 3      |

Thus we are able to compute NDCG for each rank for 3: $$NDCG(3)= \frac{DCG(3)}{IDCG(3)}=\frac{2.63}{3}=0.88$$