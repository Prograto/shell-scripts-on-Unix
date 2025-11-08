🧠 Unix Shell Script – Extract Scheme Name and NAV (AMFI Data)
🧩 Overview

This shell script downloads the latest Net Asset Value (NAV) data from AMFI (Association of Mutual Funds in India) and extracts only the following fields:

Scheme Name

Net Asset Value (NAV)

It then saves the results in a Tab-Separated Values (TSV) file named nav.tsv.

⚙️ Requirements

To run this script on Windows, you’ll need Git Bash (which provides a Unix-like terminal).

✅ Steps to install:

Download Git for Windows → https://git-scm.com/download/win

During setup, choose “Use Git Bash only” or “Use Git from Windows Terminal”.

After installation, open Git Bash.

🚀 How to Run
1️⃣ Save the script as extract_nav.sh

Create a file named extract_nav.sh and paste the following code:

#!/usr/bin/env bash
# extract_nav.sh
# This script fetches NAV data from AMFI India, extracts Scheme Name and NAV, and saves them in a TSV file.

URL="https://www.amfiindia.com/spages/NAVAll.txt"
OUTFILE="nav.tsv"

# Fetch and filter data
curl -s "$URL" | \
awk -F';' '
  $1 ~ /^[0-9]+$/ {
    scheme = $4
    nav = $5
    if (scheme != "" && nav != "")
      print scheme "\t" nav
  }
' > "$OUTFILE"

echo "✅ Data extracted and saved to $OUTFILE"

2️⃣ Make the script executable

In Git Bash, go to the folder where you saved the file and run:

chmod +x extract_nav.sh

3️⃣ Run the script
./extract_nav.sh

4️⃣ Check the output

After successful execution, a new file named nav.tsv will appear in the same folder.

You can view it by running:

head nav.tsv


📄 Example output:

Aditya Birla Sun Life Banking & PSU Debt Fund  - DIRECT - IDCW	109.8609
Aditya Birla Sun Life Banking & PSU Debt Fund  - DIRECT - MONTHLY IDCW	117.6419
Aditya Birla Sun Life Banking & PSU Debt Fund - Regular Plan-Growth	375.2609
Axis Banking & PSU Debt Fund - Direct Plan - Growth Option	2785.0785

🧠 Explanation

The script uses curl to silently download (-s) the AMFI data file.

It then pipes (|) the data to awk, which splits each line by semicolons (-F';').

Only lines starting with a numeric Scheme Code are valid data rows.

From those rows, the 4th field ($4) is the Scheme Name and the 5th field ($5) is the NAV.

The result is printed as SchemeName<TAB>NAV and saved to nav.tsv.

💡 Bonus Thought: Should This Be Stored in JSON?

While TSV is lightweight and easy to parse with command-line tools, JSON would be more suitable for structured and API-based use cases.

Format	Pros	Cons
TSV	Simple, fast to process with awk, cut, grep	No field names, poor for nested data
JSON	Structured, API-friendly, supports metadata	Slightly larger, less readable in terminal

👉 Ideally, AMFI could offer both TSV (for humans) and JSON (for systems).

🔄 Optional – Convert TSV to JSON

If you wish to convert your nav.tsv into JSON (one-liner in Git Bash):

awk -F'\t' 'BEGIN{print "["} {printf "%s{\"scheme\":\"%s\",\"nav\":%s}", (NR==1?"":","), gensub(/"/,"\\\"","g",$1), $2} END{print "]"}' nav.tsv > nav.json

✅ Expected Files After Running
extract_nav.sh   # The shell script
nav.tsv          # Extracted data (tab-separated)
README.md        # This documentation

👨‍💻 Author

Chandra Sekhar Arasavalli
B.Tech CSE | Full Stack Developer & ML Enthusiast
GitHub: Prograto
