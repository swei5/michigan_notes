[[2024-09-05]] #Regression 

### Hypothesis Testing, Recap
When conducting statistical inference, we are informally trying to answer the following question:
- Is what I’m observing real, or could it be attributable to random chance alone?

Our pursuit begins with both a question which we collect data to try to answer:
1. The **Alternative Hypothesis** is what we, the researcher, want to show
2. The **Null Hypothesis** is the opposite of what we want to show. Oftentimes, this represents the status quo. The purpose of research is often to further knowledge by refuting this commonly held belief

In order to prove a new hypothesis, one does not actually collect evidence to confirm it; rather, we proceed backwards: data is collected to **falsify the opposite** of what we want to prove. We end up using a device known as a **proof by contradiction** (falsification, refutation).
- We may **NEVER *accept*** the null hypothesis, rather, we either **refute** it or **failed to refute** it

#### Tests of Significance
This involves a statistical hypothesis test: if the null hypothesis were true, what’s the chance of observing evidence **as extreme or more extreme** than what we observed in our data if randomness alone were the explanation?

```ad-important
**Definition 4.1**: $p$-value

A $p$-value is defined to be the probability of observing a result that is **as extreme or more extreme** than what we have actually observed in our sample or experiment, **given that the null hypothesis is actually true**.
```

The $p$ -value is **NOT** the probability that null hypothesis is true. The truth of the null hypothesis is **constant** but **unknown**.

Once we compute the appropriate p-value, we need to determine if it is **substantial** enough to reject the null hypothesis.

```ad-important
**Definition 4.2**: Significance Level

The significance level of a hypothesis test, denoted as $\alpha$, is a significance threshold specified before the analysis begins such that if the $p$-value falls at or below $\alpha$, we **reject** the null hypothesis.

If $p$-value is greater than $\alpha$, then we **fail to reject** the null hypothesis since we lack substantial evidence.

Convention is to use $\alpha = 0.05$.
```

#### Types of Errors
We can characterize the types of correct or incorrect decisions in a 2 × 2 table: 
![[Pasted image 20240905122427.png|400]]

A **Type I Error** occurs if we **reject the null hypothesis** when it was actually true.
- E.g. declaring the defendant guilty when they are innocent

A **Type II Error** occurs if we **fail to reject the null hypothesis** when it was actually false.
- E.g. declaring the defendant not guilty, when they are actually guilty

Note that the significance level directly determines the Type I error rate.
- We reject $H_{0}$ when $p\le \alpha$, hence $\mathbb{P}(\text{Type 1}|H_{0}) \le \alpha$

Type II errors are that the null was incorrect is actually true, but they weren’t able to show it based on their test. For a **given significance level**, increasing sample size $n$ increases the **power** of a well-designed test, where
$$\text{power}=1-P(\text{Type II Error})$$
Generally, we try to devise tests that have the largest power possible, subject to **controlling the Type I Error** rate at $\alpha$.

In one-sample hypothesis testing, we use the $z$ -statistic when the **population standard deviation** is **known**, and use the $t$ -statistic when the **population standard deviation** is **unknown.**
$$p=\mathbb{P}(T \ge \text{t-stats} | H_{0})$$
