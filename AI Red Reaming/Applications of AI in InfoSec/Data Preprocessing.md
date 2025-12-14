# Data Preprocessing (The Cleanup Crew)

**Definition:** Data preprocessing is the art of transforming raw, messy data into a clean format that Machine Learning algorithms can actually understand.



### The 4 Pillars of Preprocessing
1.  **Cleaning:** Fixing missing values, removing duplicates, and smoothing out noise.
2.  **Transformation:** Scaling numbers (e.g., 0-1) and encoding text to math.
3.  **Integration:** Merging data from different databases.
4.  **Formatting:** Ensuring dates look like dates and numbers look like numbers.

> **Why bother?** "Garbage In, Garbage Out." If your data has invalid IPs or impossible numbers, your AI will learn incorrect patterns.

---

## 1. Detecting "Bad Data" (Invalid Values) 🕵️‍♂️

Before we fix data, we must find the errors. Using our Network Log dataset as an example, here is how to validate specific columns.

### A. Validating IP Addresses
**Rule:** Must match the standard IPv4 format (e.g., `192.168.1.1`).
**Method:** Use **Regular Expressions (Regex)**.

```python
import re

def is_valid_ip(ip):
    # Regex pattern for a valid IPv4 address
    pattern = re.compile(r'^((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$')
    return bool(pattern.match(str(ip)))

# Find rows where 'is_valid_ip' returns False
invalid_ips = data[~data['source_ip'].apply(is_valid_ip)]
````

### B. Validating Ports

**Rule:** Port numbers must be integers between **0 and 65535**.

```python
def is_valid_port(port):
    try:
        port = int(port)
        return 0 <= port <= 65535
    except ValueError:
        return False # It wasn't a number
```

### C. Validating Protocols

**Rule:** Must belong to a list of known standard protocols.

```python
valid_protocols = ['TCP', 'TLS', 'SSH', 'POP3', 'DNS', 'HTTPS', 'SMTP', 'FTP', 'UDP', 'HTTP']

# Check if the protocol is NOT in our valid list
invalid_protocols = data[~data['protocol'].isin(valid_protocols)]
```

### D. Validating Bytes & Threat Levels

**Rule:**

  * **Bytes:** Must be a positive number.
  * **Threat Level:** Must be 0, 1, or 2.

-----

## 2\. Handling the Bad Data 🧹

Once you have identified the garbage, you have two choices: **Drop it** or **Fix it**.

### Strategy A: Dropping (The "Nuclear" Option)

Delete any row that contains bad data.

  * **Pros:** Guarantees your data is 100% real/clean.
  * **Cons:** You might lose too much data. (In this dataset, dropping reduced us to only 77 entries\!).

<!-- end list -->

```python
# Drop rows with invalid IPs, Ports, etc.
# errors='ignore' prevents crashing if the index is already gone
data = data.drop(invalid_ips.index, errors='ignore') 
data = data.drop(invalid_ports.index, errors='ignore')
```

### Strategy B: Imputing (The "Repair" Option)

Instead of deleting, we fill in the gaps with educated guesses.

**Step 1: Standardize to `NaN`**
First, turn all "weird" errors (like `?`, `MISSING_IP`, `string_port`) into a standard `NaN` (Not a Number) value. This makes Pandas understand that the data is missing.

```python
import numpy as np

# Replace all known garbage strings with NaN
bad_tokens = ['INVALID_IP', 'STRING_PORT', 'NON_NUMERIC', '?']
df.replace(bad_tokens, np.nan, inplace=True)

# Force columns to be numbers (non-numbers become NaN automatically)
df['destination_port'] = pd.to_numeric(df['destination_port'], errors='coerce')
```

**Step 2: Simple Imputation (Basic Guessing)**

  * **Numeric Data:** Fill with the **Median** (middle value) or Mean.
  * **Categorical Data:** Fill with the **Mode** (Most Frequent value).

<!-- end list -->

```python
from sklearn.impute import SimpleImputer

# Fill numbers with Median
num_imputer = SimpleImputer(strategy='median')
df[['destination_port', 'threat_level']] = num_imputer.fit_transform(df[['destination_port', 'threat_level']])

# Fill text with Most Frequent
cat_imputer = SimpleImputer(strategy='most_frequent')
df[['protocol']] = cat_imputer.fit_transform(df[['protocol']])
```

**Step 3: Advanced Imputation (KNN)**

**K-Nearest Neighbors (KNN)** looks at similar rows to make a better guess.

  * *Concept:* "This row looks like a High Threat attack based on the other columns, so the missing Port is probably 80."

<!-- end list -->

```python
from sklearn.impute import KNNImputer

# Use 5 nearest neighbors to guess the missing value
knn_imputer = KNNImputer(n_neighbors=5)
df[numeric_cols] = knn_imputer.fit_transform(df[numeric_cols])
```

-----

## 3\. Domain-Specific Rules 🧠

Sometimes, math isn't enough. You need logic based on the specific field (Cybersecurity).

  * **Missing IP?** $\rightarrow$ Set to `0.0.0.0` (Standard placeholder).
  * **Port out of range?** $\rightarrow$ Clip it to `65535` (Max limit).
  * **Unknown Protocol?** $\rightarrow$ Assume the most common one (`TCP`).

<!-- end list -->

```python
# Logic-based cleaning
df['source_ip'] = df['source_ip'].fillna('0.0.0.0')
df['destination_port'] = df['destination_port'].clip(lower=0, upper=65535)
```

```
```
