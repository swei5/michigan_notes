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

### Content-based Filtering