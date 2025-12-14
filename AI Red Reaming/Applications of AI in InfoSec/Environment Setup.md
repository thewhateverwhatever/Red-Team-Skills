

# 🐍 Miniconda & Environment Setup (The "Starter Kit")

**Definition:** Miniconda is a **minimal installer** for Python.
* **Miniconda:** Lightweight. Just Python + `conda` manager. You add what you need.
* **Anaconda:** Heavyweight. Python + `conda` + 700+ pre-installed libraries (NumPy, Pandas, etc.).

**Why choose Miniconda?**
1.  **🚀 Performance:** Optimized packages often run faster.
2.  **📦 Package Management:** `conda` handles complex dependency chains (crucial for Deep Learning) better than standard tools.
3.  **🛡️ Isolation:** Keeps projects separate. Project A (Python 3.9) won't break Project B (Python 3.12).

---

## 1. Installation Guide 🛠️

### A. Windows (via Scoop)
Using **Scoop** makes management easier than the standard installer.

1.  **Install Scoop** (PowerShell):
    ```powershell
    Set-ExecutionPolicy RemoteSigned -scope CurrentUser
    irm get.scoop.sh | iex
    ```
2.  **Add Extras Bucket:**
    ```powershell
    scoop bucket add extras
    ```
3.  **Install Miniconda:**
    ```powershell
    scoop install miniconda3
    ```
4.  **Verify:** Restart terminal and run `conda --version`.

### B. macOS (via Homebrew)
1.  **Install Homebrew** (if needed):
    ```bash
    /bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))"
    ```
2.  **Install Miniconda:**
    ```bash
    brew install --cask miniconda
    ```
3.  **Verify:** Restart terminal and run `conda --version`.

### C. Linux (Manual Script)
1.  **Download & Install:**
    ```bash
    wget [https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh](https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh)
    chmod +x Miniconda3-latest-Linux-x86_64.sh
    ./Miniconda3-latest-Linux-x86_64.sh -b -u
    ```
2.  **Load Shell:**
    ```bash
    eval "$(/home/$USER/miniconda3/bin/conda shell.$(ps -p $$ -o comm=) hook)"
    ```
3.  **Verify:** `conda --version`.

---

## 2. Configuration & Initialization ⚙️

### Step 1: Initialize Shell
Make your terminal recognize `conda` commands permanently.
```bash
conda init
# Close and reopen your terminal after this!
````

### Step 2: Configure Channels

Tell Conda where to download packages from (Priority: Strict).

```bash
conda config --add channels defaults
conda config --add channels conda-forge
conda config --add channels nvidia      # Only if you have an NVIDIA GPU
conda config --add channels pytorch
conda config --set channel_priority strict
```

### Step 3: Stop Auto-Activation

By default, Conda activates the `(base)` environment on startup. To stop this:

```bash
conda config --set auto_activate_base false
```

-----

## 3\. Managing Virtual Environments 🌐

**Why?** To prevent "Dependency Hell" (conflicts between projects).

### Create an Environment

Create a clean room named `ai` using Python 3.11:

```bash
conda create -n ai python=3.11
```

### Activate / Deactivate

  * **Enter the room:**
    ```bash
    conda activate ai
    # Prompt changes to: (ai) user@host $
    ```
  * **Leave the room:**
    ```bash
    conda deactivate
    ```

-----

## 4\. Essential AI Packages 📦

Install the core stack inside your active `(ai)` environment.

### A. Core Data Science & ML (via Conda)

Installs NumPy, Pandas, Scikit-Learn, Transformers, etc.

```bash
conda install -y numpy scipy pandas scikit-learn matplotlib seaborn transformers datasets tokenizers accelerate evaluate optimum huggingface_hub nltk category_encoders
```

### B. PyTorch (Deep Learning)

Installs PyTorch with CUDA support (for NVIDIA GPUs).

```bash
conda install -y pytorch torchvision torchaudio pytorch-cuda=12.4 -c pytorch -c nvidia
```

### C. Utilities (via Pip)

Use `pip` for tools not found in Conda channels.

```bash
pip install requests requests_toolbelt
```

-----

## 5\. Maintenance 🔄

To update all packages managed by Conda within your current environment:

```bash
conda update --all
```

*Note: This does **not** update pip-installed packages.*

```
```
