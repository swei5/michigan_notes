[[2023-11-21]] #UnsupervisedLearning

### Introduction
Recommender Systems predict what you'll like - they predict your preferences.
- Netflix shows, Spotify, Amazon products, etc.

It is extremely difficult: 
- Users have very little tolerance for adding preference data
- System knows almost nothing about you
- Everyone is different

There are two approaches to data collection.
- Explicit data collection
	- Ask users to rate movies, products, whatever
- Implicit data collection
	- Web logs of past user activity

However, it's often seen that traditional ML doesn't work here since product qualities are **difficult to extract**.

This introduces [[19 Collaborative Filtering|collaborative filtering]].

### User-based Collaborative Filtering
We want to recommend content enjoyed *people who are similar to you*. The assumption is *people who agreed in the past will agree in the future*.

#### Averaging
Simply fill in the missing score by the **average for each item** over all users.

![[Pasted image 20231121105721.png|400]]

However, averaging ignores **uniqueness** of a user, and will be terrible when there is large variation in interest. A better way to do this is **nearest neighbor algorithm**.

#### Nearest Neighbor
Finding another user with similar properties and use their rating to "fill in the blank". There are three main ways to measure the *distance* between neighbors. ^d9da9a
1. Euclidean distance
	- Each content is a dimension in the vector space
	- Similar to [[15 Info Retrieval I - Text Analysis#^a3a539|vector space model]]
2. Cosine similarity
	- Each content is a dimension in the vector space
	- Consider only the angle between two vectors, **NOT** the length
3. Pearson correlation coefficient
	- Formula and background introduced [[19 Collaborative Filtering#^723dd1|here]] before

#### $k$ -NN
Even better, we want to select $k$ nearest neighbors, then select the most frequent (mode) score.

**Problem**: The popular film will be recommended most of the time because it will often be the **most frequent** score; however, it might not always be the best choice for a user
**Solution**: Weighted $k$ -NN
$$r_{ui}=\frac{\sum_{v\in KNN(u)}\text{sim}(u,v) r_{v,i}}{\sum_{v\in KNN(u)}|\text{sim}(u,v)|}$$
Here, $k=\sum_{v\in KNN(u)}|\text{sim}(u,v)|$ is a **normalizer**. Notice the similarity to [[19 Collaborative Filtering#^72b25f|this formula seen earlier]].

**Problem**: How would a "cult classic" film affect recommendations?
- Enjoyed by a few, who REALLY like it
- Rare film less likely to be predicted by $k$ -NN
**Solution**: Can use inverse-user-frequency
- Similar to [[15 Info Retrieval I - Text Analysis#^5975fc|inverse document frequency]]
	- Rarely seen movies are more influential

```ad-summary
**Collaborative Filtering, Weaknesses**
- Cold start  
	- Need user data before you can start making accurate recommendations
- Scalability  
	- Millions of users, $n$-choose-2 pairs  
	- Large amount of computation power
- Sparsity  
	- Most users will only have rated a small subset of the overall database  
	- Even the most popular items have very few ratings
```
### Content-based Filtering
The solution to the above problems is **content-based filtering**, by which we recommend items **similar to items** the user has liked in the past. The assumption is that *people will like the same things in the future that they liked in the past*.

In content-based filtering, we use [[21 Recommender System#^d9da9a|similar techniques]] to measure distance as in user-based filtering.

**Problem**: Website doesn't know all your preferences, and there is lost opportunity to introduce you to **new content**.
**Solution**: Hybrid filtering - combine user-based and content-based filtering.