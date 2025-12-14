# 📊 Datasets & Data Preprocessing (The Foundation)

**Definition:** A Dataset is a collection of information used to train AI.
* **Why it matters:** "Garbage In, Garbage Out." If your data is bad (messy, biased, incomplete), your AI will be bad, no matter how fancy the algorithm is.

---

## 1. Types of Data 🗂️

| Type | Description | Example |
| :--- | :--- | :--- |
| **Tabular** | Organized in rows/columns (like Excel). | Sales records, Employee DB. |
| **Image** | Pixel arrays. | Photos, Medical X-rays. |
| **Text** | Unstructured words/sentences. | Emails, Books, Tweets. |
| **Time Series** | Data points over time. | Stock prices, Weather history. |

---

## 2. What Makes a Dataset "Good"? ✅

A high-quality dataset must hit these 6 benchmarks:

1.  **Relevance:** Does the data actually matter? (Don't use stock prices to predict the weather).
2.  **Completeness:** Are there blanks? (Too many missing values = weak patterns).
3.  **Consistency:** Is the format the same? (Don't mix "Jan 1, 2023" with "01-01-23").
4.  **Quality:** Is it accurate? (No typos or transmission errors).
5.  **Representativeness:** Does it cover everyone? (A Face ID dataset with only men will fail on women).
6.  **Balance:** Are the classes equal? (If 99% of your data is "Cat" and 1% "Dog", the AI will just guess "Cat" every time).

---

## 3. Case Study: Network Logs (`demo_dataset.csv`) 📁

We will use a sample dataset of network traffic to build an Intrusion Detection System.

**Columns (Features):**
* `log_id`: Unique ID.
* `source_ip`: Where the traffic came from.
* `destination_port`: Where it's going.
* `protocol`: Type of traffic (TCP, SSH, etc.).
* `bytes_transferred`: Size of data.
* `threat_level`: **The Target**. (0 = Safe, 1 = Low Threat, 2 = High Threat).

**Known Issues (Challenges):**
* Mixed data types (numbers and text).
* Missing values ("NaN").
* Weird symbols in the threat column ("?", "-1").

---

## 4. Loading & Exploring with Pandas 🐼

We use the Python library **Pandas** to handle tabular data. It loads data into a **DataFrame** (a programmable Excel sheet).

### A. Loading the Data
```python
import pandas as pd

# Load the CSV file into a variable called 'data'
data = pd.read_csv("./demo_dataset.csv")
````

### B. The "First Look" (Sanity Check)

Always look at your data before doing math on it.

**1. See the first 5 rows:**

```python
print(data.head())
# Checks: Do the columns look right? Is the data loaded correctly?
```

**2. Check Data Types & Size:**

```python
print(data.info())
# Checks: Is 'bytes_transferred' a number or text? How many rows total?
```

**3. Find Missing Data:**

```python
print(data.isnull().sum())
# Checks: Which columns have empty cells (NaN)?
# Output Example:
# source_ip: 0
# protocol: 50 (This means 50 rows have no protocol!)
```

```
```
