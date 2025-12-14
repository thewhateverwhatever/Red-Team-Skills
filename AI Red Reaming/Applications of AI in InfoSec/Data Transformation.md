<img width="3998" height="2021" alt="image" src="https://github.com/user-attachments/assets/ca307aed-ad92-4052-9dac-bbba3d837f36" /><img width="3998" height="2021" alt="image" src="https://github.com/user-attachments/assets/87b51b2c-5656-44b5-bb20-d1c33509d47e" /><img width="3998" height="2021" alt="image" src="https://github.com/user-attachments/assets/099d2f3f-ea74-447f-a23e-2751c5c8a844" />
# Data Transformation (The Polishing Phase)

**Definition:** Data transformation improves the way features are represented to make them easier for Machine Learning models to digest.
* **Why do it?** Models struggle with text and massively lopsided numbers. Transformations convert text to math and smooth out extreme outliers.

---

## 1. Encoding Categorical Features 🔠➡️🔢

Machine Learning algorithms cannot understand words like "Red" or "TCP". They only understand numbers. **Encoding** fixes this.

### Common Techniques
* **OneHotEncoder:** Best for categories without order (e.g., Colors, Protocols).
* **LabelEncoder:** Best for ordered categories (e.g., Small, Medium, Large $\rightarrow$ 0, 1, 2). *Warning: Using this on unordered data might confuse the model into thinking "Blue" > "Red".*
* **HashingEncoder:** Best for categories with thousands of unique values.

### Deep Dive: One-Hot Encoding
This method creates a new binary column (0 or 1) for every possible category.

**Example: The `Color` Feature**
Original Data:
| ID | Color |
| :--- | :--- |
| 1 | Red |
| 2 | Green |



Transformed Data:
| ID | Color_Red | Color_Green | Color_Blue |
| :--- | :--- | :--- | :--- |
| 1 | **1** | 0 | 0 |
| 2 | 0 | **1** | 0 |

**Code Implementation (Encoding Protocol):**
```python
from sklearn.preprocessing import OneHotEncoder

# Create the encoder
encoder = OneHotEncoder(handle_unknown='ignore', sparse_output=False)

# Fit and transform the 'protocol' column
encoded = encoder.fit_transform(df[['protocol']])

# Create a DataFrame with nice column names (e.g., protocol_TCP, protocol_UDP)
encoded_df = pd.DataFrame(encoded, columns=encoder.get_feature_names_out(['protocol']))

# Attach new columns to original data and remove the old text column
df = pd.concat([df.drop('protocol', axis=1), encoded_df], axis=1)
````

-----

## 2\. Handling Skewed Data 📉

**Problem:** "Skewed" data means the values are lopsided.

  * *Example:* In a dataset of file sizes, most files are 1KB to 5KB, but one file is 10GB.
  * **Effect:** The 10GB outlier dominates the training, forcing the model to ignore the small files.

**Solution: Log Transformation**
We squash the giant numbers down using a logarithm. This makes the distribution look more like a "Bell Curve" (Normal Distribution), which models love.

  * **Technique:** `np.log1p` (Log Plus One).
  * *Why +1?* Log(0) is mathematically undefined (error). Adding 1 ensures the code never crashes on zero values.

<!-- end list -->

```python
import numpy as np

# Apply Log transformation to 'bytes_transferred'
df["bytes_transferred"] = np.log1p(df["bytes_transferred"]) 
```

-----

## 3\. Data Splitting (The Final Exam) ✂️

You must split your data to prove your model actually works and didn't just memorize the answers.

**The Golden Ratio (60 / 20 / 20):**

1.  **Training Set (60%):** The study material. The model learns from this.
2.  **Validation Set (20%):** The practice quiz. Used to tune settings (hyperparameters) and check progress during training.
3.  **Test Set (20%):** The Final Exam. Completely hidden until the very end to check true performance.

**Code Implementation:**
We perform the split in two steps to get three sets.

```python
from sklearn.model_selection import train_test_split

# Step 1: Separate Input (X) and Answer Key (y)
X = df.drop("threat_level", axis=1)
y = df["threat_level"]

# Step 2: First Split (80% Train+Val, 20% Test)
X_train_full, X_test, y_train_full, y_test = train_test_split(X, y, test_size=0.2, random_state=1337)

# Step 3: Second Split (Take 25% of the 80% to get 20% Validation)
# 0.80 * 0.25 = 0.20 (Math works out!)
X_train, X_val, y_train, y_val = train_test_split(X_train_full, y_train_full, test_size=0.25, random_state=1337)

# Result:
# X_train = 60%
# X_val   = 20%
# X_test  = 20%
```

```
```
