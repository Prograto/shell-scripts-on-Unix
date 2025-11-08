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


### Step 2️⃣ — Make the script executable

Open Git Bash, navigate to the folder where your file is saved, and run:

chmod +x extract_nav.sh

### Step 3️⃣ — Run the script

Execute the script by running:

./extract_nav.sh

### Step 4️⃣ — Verify the output

After execution, a file named nav.tsv will be created in the same folder.
View the first few lines with:

head nav.tsv

📊 Sample Output (nav.tsv)
Aditya Birla Sun Life Banking & PSU Debt Fund  - DIRECT - IDCW	109.8609
Aditya Birla Sun Life Banking & PSU Debt Fund  - DIRECT - MONTHLY IDCW	117.6419
Aditya Birla Sun Life Banking & PSU Debt Fund - Regular Plan-Growth	375.2609
Axis Banking & PSU Debt Fund - Direct Plan - Growth Option	2785.0785

🧠 Explanation

The script uses curl to silently (-s) download the AMFI data file.

Each line in the downloaded text is semicolon-separated (;).

Using awk, the script:

Filters lines that begin with a numeric scheme code.

Extracts only the 4th field (Scheme Name) and the 5th field (NAV).

Outputs them as tab-separated values (\t).

The resulting clean dataset is saved to nav.tsv.

💬 Discussion: Should This Be Stored in JSON?

While TSV (Tab-Separated Values) is convenient for command-line tools and spreadsheets,
JSON (JavaScript Object Notation) is more flexible and machine-readable.

Format	Advantages	Disadvantages
TSV	Lightweight, human-readable, easy with awk/cut	No structure or field names
JSON	Structured, hierarchical, API-friendly	Slightly larger, less readable for humans
Example JSON representation
[
  {
    "scheme": "Aditya Birla Sun Life Banking & PSU Debt Fund - Direct - IDCW",
    "nav": 109.8609
  },
  {
    "scheme": "Axis Banking & PSU Debt Fund - Direct Plan - Growth Option",
    "nav": 2785.0785
  }
]

🔄 Optional: Convert TSV to JSON

To convert your nav.tsv file to nav.json directly in Git Bash, run:

awk -F'\t' 'BEGIN{print "["} {
  printf "%s{\"scheme\":\"%s\",\"nav\":%s}", (NR==1?"":","), gensub(/"/,"\\\"","g",$1), $2
} END{print "]"}' nav.tsv > nav.json

✅ Expected Files After Execution
extract_nav.sh   # Shell script
nav.tsv          # Output file containing Scheme Name and NAV
README.md        # Documentation (this file)

🧩 Sample Submission Structure
.
├── extract_nav.sh
├── nav.tsv
└── README.md

👨‍💻 Author

Chandra Sekhar Arasavalli
B.Tech (CSE) | Full Stack Developer & ML Enthusiast
GitHub: Prograto
