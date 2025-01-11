[[2025-01-10]] #DataMining

### Intro
**Data mining** is the process of identifying valid, novel, potentially useful, and ultimately understandable patterns in data.

The data mining process:
1. Selection
2. Preprocessing (80% of work)
3. **Mining**
4. Interpretation and evaluation

---
### Data
Data is a collection of data objects and their attributes.
- **Attribute**: a property or characteristic of an object
	- Also known as variable, field, characteristic, or feature
- **Data Object**: A collection of attributes that describe it
	- Also known as record, data point, case, sample, or instance

#### Attributes
**Attribute Values** are numbers or symbols assigned to an attribute.
- Same attribute can be mapped to diff attribute values
- Diff attributes can be mapped to the same set of values

There are four main types of attributes:
- Nominal – categorical values only
- Ordinal - ordered values
- Interval - meaningful differences
	- **0** is **NOT LOGICAL**
- Ratio - meaningful differences + ratio

The type of an attribute depends on which of the following properties it possesses:
- Nominal attribute: **Distinctness**: $=$, $\ne$
- Ordinal attribute: **Order**: $>$, $<$
- Interval attribute: **Addition**: $+$, $-$
- Ratio attribute: **Multiplication**: $*$, $/$

![[Pasted image 20250110150820.png|500]]

Another way to categorize attributes is by **discrete** and **continuous**.
- **Discrete Attribute**
	- Has only a **finite** or countably infinite set of values
- **Continuous Attribute**
	- Has **real** numbers as attribute values

#### Types of data sets
There are three broad types of data sets.
- **Record**
	- Data Matrix
		- If data records have the **same fixed set of numeric attributes**, then the data objects can be thought of as points in a multi-dimensional space, where each dimension represents a distinct attribute
	- Document Data
		- Each term is a component (attribute) of the vector
	- Transaction Data
		- Each record (transaction) involves a set of items
- **Graph**
	- World Wide Web
	- Molecular Structures
- **Ordered**
	- Spatial Data
	- Temporal Data
	- Sequential Data
	- Genetic Sequence Data

#### Data Quality
**Noise** refers to **modification** of original values.
- E.g. distortion of a person’s voice when talking on a poor phone and “snow” on television screen

**Outliers** are data objects with characteristics that are **considerably different** than most of the other data objects in the data set.

Data set may include records that are **missing**, **duplicates**, or almost duplicates of one another.

Data cleaning process deals with duplicate data issues, finds and corrects measurement error (noise+outliers), and deals with missing values.

---
### Data Transformation
**Aggregation** is combining two or more attributes (or objects) into a single attribute (or object).
- **Data reduction**: Reduce the number of attributes or objects
- **Change of scale**: E.g. cities aggregated into regions, states, countries, etc
- **More “stable” data**: Aggregated data tends to have less variability

**Feature creation** is creating new attributes that can capture the important information in a data set much **more effectively** than the original attributes.

There are three general methodologies:
- **Feature Extraction**: domain-specific
- **Feature Construction**: combining features
- *Mapping data to new space (embeddings)*
	- **Attribute** **transformation**: function $f$ that maps the values of a given attribute to a new set of replacement values such that each old value can be identified with one of the new values
		- Simple functions
		- Standardization, Normalization
		- Smoothing
		- Fourier transform
		- Wavelet transform

##### Standardization
Consider $D$ of $n$ observations over $m$ variables ($n \times m$ matrix). We say $D$ is **zero-centered** if $\text{mean}(d_{i})=0$ for each column $d_{i}$ of $D$

We can center any dataset by **subtracting the mean** from each column.

```ad-warning
However, note that
- Centering cannot be applied to all sorts of data
- It destroys non-negativity
- Centered data won’t contain integers
- Centering destroys sparsity
```

An attribute $d$ has unit variance if $\text{var}(d)=1$. A dataset $D$ has unit variance iff $\forall d_{i} \text{var}(d_{i})=1$

We obtain unit variance by **dividing every column by its standard deviation**.

Division by standard deviation makes **all observations equally important**.

```ad-warning
However note that
- Dividing by standard deviation is based on the assumption that the values follow a **Gaussian distribution**
	- Often this is plausible: Law of Large Numbers, Central Limit Theorem, etc.
- Not all data is Gaussian
- Not all data distributions **have** a mean
	- Power-law distributions
```

Data that is **zero centered** and has **unit** **variance** is called **standardized**, or the $z$ -scores.
- Many methods (implicitly) assume data is standardized, e.g. PCA

Of course, we may apply non-linear transformations to the data before standardizing.

---
### Data Selection, Reduction
Sampling is the main technique employed for data selection. It is often used for both the preliminary investigation of the data and the final data analysis. Sampling is used in data mining because processing the entire set of data of interest is too expensive or time consuming.

Using a sample will work almost as well as using the entire data set, if the sample is **representative**.
- A sample is representative if it has approximately the same property (of interest) as the original set of data

There are four main types of sampling:
- **Simple Random Sampling**
	- Equal probability of selecting any particular item
- **Sampling without replacement**
	- Each selected item is removed from the population
- **Sampling with replacement**
- **Stratified sampling**
	- Split the data into several partitions; then draw random samples from each partition

#### Dimensionality Reduction
When dimensionality increases, data becomes **increasingly sparse** in the space that it occupies. Definitions of density and distance between points, which is critical for clustering and outlier detection, become **less meaningful**.

![[Pasted image 20250110160222.png|400]]

The purpose of dimensionality reduction is to
- Avoid curse of dimensionality
- Reduce amount of time and memory required by data mining algorithms
- Allow data to be more easily visualized
- May help to eliminate irrelevant features or reduce noise

#### Feature Subset Selection
Another way to reduce dimensionality of data - picking a subset of the features. It gets rid of
- **Redundant features**
	- Duplicate much or all of the information contained in one or more attributes
- **Irrelevant features**
	- Contain no information that is useful for the data mining task at hand – removing them leads to better results

There are four main methods to conduct this:
- **Brute-force approach**
- **Embedded approaches**
	- Feature selection occurs naturally as part of the data mining algorithm (e.g., regularization)
- **Filter approaches**
	- Features are selected before data mining algorithm is run (external quality measure)
- **Wrapper approaches**
	- Use the data mining algorithm as a black box to find best **subset** of attributes (specific to your method, through some search algorithm)