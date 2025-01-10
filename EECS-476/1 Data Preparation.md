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