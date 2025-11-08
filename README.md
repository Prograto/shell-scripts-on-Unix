🧠 Unix Shell Script – Extract Scheme Name and NAV (AMFI Data)
🧩 Overview

This shell script downloads the latest Net Asset Value (NAV) data from AMFI (Association of Mutual Funds in India) and extracts only two fields:

Scheme Name

Net Asset Value (NAV)

It saves the results in a Tab-Separated Values (TSV) file named nav.tsv.

⚙️ Requirements

To run this script on Windows, you’ll need Git Bash (which provides a Unix-like terminal).

✅ Install Git Bash:

Download Git for Windows → https://git-scm.com/download/win

During installation, choose “Use Git Bash only” or “Use Git from Windows Terminal”.

After installation, open Git Bash.

🚀 How to Run the Script in Git Bash
1️⃣ Save the script as extract_nav.sh

Create a new file named extract_nav.sh and paste the code below 👇

🧾 Script: extract_nav.sh
#!/usr/bin/env bash
# extract_nav.sh
# This script fetches NAV data from AMFI India, extracts Scheme Name and NAV,
# and saves them in a TSV file.

# URL containing mutual fund NAV data
URL="https://www.amfiindia.com/spages/NAVAll.txt"

# Output file name
OUTFILE="nav.tsv"

# Fetch the data and extract required fields
curl -s "$URL" | \
awk -F';' '
  # Process only valid lines (where first field is numeric)
  $1 ~ /^[0-9]+$/ {
    scheme = $4
    nav = $5
    if (scheme != "" && nav != "")
      print scheme "\t" nav
  }
' > "$OUTFILE"

# Print completion message
echo "✅ Data extracted and saved to $OUTFILE"

2️⃣ Make the script executable

Open Git Bash, navigate to the folder where the script is saved, and run:

chmod +x extract_nav.sh

3️⃣ Run the script
./extract_nav.sh

4️⃣ View the output file

The script creates a file named nav.tsv in the same folder.
To see the first few lines:

head nav.tsv

📄 Sample Output (nav.tsv)
Aditya Birla Sun Life Banking & PSU Debt Fund  - DIRECT - IDCW	109.8609
Aditya Birla Sun Life Banking & PSU Debt Fund  - DIRECT - MONTHLY IDCW	117.6419
Aditya Birla Sun Life Banking & PSU Debt Fund - Regular Plan-Growth	375.2609
Axis Banking & PSU Debt Fund - Direct Plan - Growth Option	2785.0785

🧠 Explanation

The script uses curl to silently download (-s) the AMFI data file.

It then pipes (|) the data to awk, which splits each line using semicolons (-F';').

Only lines starting with a numeric Scheme Code are valid NAV records.

The 4th field ($4) contains the Scheme Name, and the 5th field ($5) contains the NAV (Net Asset Value).

The result is printed as SchemeName<TAB>NAV and saved to nav.tsv.

💬 Why JSON Might Be Better

Although TSV is easy for quick command-line parsing, JSON offers richer structure and is more suitable for data exchange or APIs.

Format	Pros	Cons
TSV	Simple, fast, human-readable	Lacks structure and field names
JSON	Structured, API-friendly, supports metadata	Larger, less readable in terminal
Example JSON structure (same data in JSON)
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

🔄 Optional – Convert TSV to JSON (Bonus)

If you want to convert your nav.tsv file into JSON directly in Git Bash, run:

awk -F'\t' 'BEGIN{print "["} {
  printf "%s{\"scheme\":\"%s\",\"nav\":%s}", (NR==1?"":","), gensub(/"/,"\\\"","g",$1), $2
} END{print "]"}' nav.tsv > nav.json


This will create a nav.json file in the same folder.

✅ Expected Files After Execution
extract_nav.sh   # Shell script
nav.tsv          # Extracted Scheme Name and NAV data
README.md        # Documentation

👨‍💻 Author

Chandra Sekhar Arasavalli
B.Tech CSE | Full Stack Developer & ML Enthusiast
GitHub: Prograto
