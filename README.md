# 📄 UNIX Shell Script Assignment  
### 🧠 **Task:** Extract Scheme Name and NAV from AMFI Data

---

## 🧩 **Overview**

This project demonstrates a simple **Unix shell script** that downloads the latest **Net Asset Value (NAV)** data from the official AMFI (Association of Mutual Funds in India) website and extracts only the required fields:

- **Scheme Name**  
- **Net Asset Value (NAV)**  

The processed output is saved as a **Tab-Separated Values (TSV)** file named `nav.tsv`.

---

## ⚙️ **Requirements**

To execute this script on **Windows**, you will need a Unix-like shell environment such as **Git Bash**.

### 🧰 Install Git Bash
1. Download from: [https://git-scm.com/download/win](https://git-scm.com/download/win)  
2. During installation, choose:
   - ✅ *Use Git Bash only* or  
   - ✅ *Integrate Git Bash with Windows Terminal*
3. After installation, open **Git Bash**.

---

## 🚀 **How to Run the Script (in Git Bash)**

### Step 1️⃣ — Create the script file
Open any text editor (like Notepad) and paste the following script.  
Save it as **`extract_nav.sh`** in your working folder (e.g., `Downloads` or `Desktop`).

---

### 🧾 **Shell Script: `extract_nav.sh`**

```bash
#!/usr/bin/env bash
# extract_nav.sh
# This script fetches NAV data from AMFI India, extracts Scheme Name and NAV fields,
# and saves them to a tab-separated file (nav.tsv).

# URL for AMFI NAV data
URL="https://www.amfiindia.com/spages/NAVAll.txt"

# Output file
OUTFILE="nav.tsv"

# Fetch and process data
curl -s "$URL" | \
awk -F';' '
  # Process lines where first field is numeric (scheme code)
  $1 ~ /^[0-9]+$/ {
    scheme = $4
    nav = $5
    if (scheme != "" && nav != "")
      print scheme "\t" nav
  }
' > "$OUTFILE"

echo "✅ Data extracted successfully and saved to $OUTFILE"
