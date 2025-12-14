
# 🪐 JupyterLab (The Interactive Data Lab)

**Definition:** JupyterLab is a web-based **Interactive Development Environment (IDE)**.
* **What it does:** It lets you write code, run it, see the results (graphs/tables), and write notes about it—all in one document.
* **Who uses it:** It is the standard tool for Data Scientists and Machine Learning engineers because it supports "trial and error" experimentation.



---

## 1. Why JupyterLab? 🧪

1.  **Interactive:** Run code in small chunks (cells) instead of running the whole script at once.
2.  **Visual:** See your graphs (`matplotlib`) and data tables (`pandas`) directly below the code that created them.
3.  **Documentation:** Combine code with rich text (Markdown) to explain your logic to others.

---

## 2. Installation & Launch 🚀

**Prerequisite:** Ensure your `(ai)` Conda environment is active.

### Install
Run this in your terminal:
```bash
conda install -y jupyter jupyterlab notebook ipykernel
````

### Launch

Start the server with:

```bash
jupyter lab
```

  * This will open a new tab in your web browser (e.g., Chrome/Firefox) showing the JupyterLab interface.

-----

## 3\. Using Notebooks 📓

The core file type is the **Notebook** (`.ipynb`). It is made of **Cells**.

### Types of Cells

  * **Code Cells:** For Python code.
  * **Markdown Cells:** For text, headers, and notes (like this document).

### Basic Workflow

1.  **Create:** Click the "Python 3" icon in the Launcher.
2.  **Write Code:** Type `print("Hello AI")` in a cell.
3.  **Run:** Press `Shift + Enter` to execute the cell.
      * *Result:* The output appears instantly below the cell.

### Adding & Saving

  * **Add Cell:** Click the **`+`** button in the toolbar.
  * **Change Type:** Use the dropdown menu to switch between "Code" and "Markdown".
  * **Save:** Click the 💾 icon or press `Ctrl + S`. (Right-click the tab to Rename).

-----

## 4\. The Concept of "State" 🧠

Jupyter is **Stateful**. It remembers everything you run.

  * **How it works:** If you define `x = 5` in Cell 1 and run it, Cell 100 knows that `x` is 5.
  * **The Trap:** If you run cells **out of order**, things get confusing.
    1.  Cell A: `x = 1` (Run this)
    2.  Cell B: `print(x)` -\> Output: `1`
    3.  Change Cell A to `x = 2` (Run this)
    4.  Run Cell B again -\> Output: `2`

> **Note:** Variables persist as long as the **Kernel** (the brain) is running, regardless of where the cell is located in the notebook.

-----

## 5\. Visualizing Data Example 📊

Jupyter shines when plotting data. Here is a sample block you can paste into a notebook:

```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

# 1. Create fake data
data = pd.DataFrame({
    "column1": np.random.rand(50),          # 50 random numbers
    "column2": np.random.rand(50) * 10      # 50 random numbers x 10
})

# 2. Show the data table
print(data.head())

# 3. Draw the graph
plt.scatter(data["column1"], data["column2"])
plt.xlabel("Column 1")
plt.ylabel("Column 2")
plt.title("Scatter Plot Example")
plt.show()
```

-----

## 6\. Restarting the Kernel 🔄

Sometimes your variables get messy, or the code freezes. You need a "Soft Reboot."

**The Kernel** is the background process running your Python code.

  * **Restart Kernel:** Wipes all variables from memory (like restarting your computer) but keeps the code and outputs on the screen.
  * **Restart & Clear Output:** Wipes memory AND clears all the graphs/text outputs, giving you a completely fresh sheet.

**How to do it:**

1.  Click **Kernel** in the top menu.
2.  Select **Restart Kernel...**
3.  *Important:* You must re-run your import cells (e.g., `import pandas`) after a restart\!

<!-- end list -->

```
```
