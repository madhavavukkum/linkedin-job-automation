# Job Scraper Workflow (n8n + Google Sheets)

This project is an **automated job extraction and tracking workflow** built using [n8n](https://n8n.io/).  
It fetches job listings, parses attributes (title, company, location, description, etc.), scores them, and stores the results in **Google Sheets**.  
The workflow also supports **upsert** mode (update existing rows or insert new ones) to avoid duplicates.

---

## 📸 Screenshots

### Workflow
![Workflow](./workflow.jpg)

### Google Sheet 1
![Sheet 1](./sheet1.jpg)

### Google Sheet 2
![Sheet 2](./sheet2.jpg)


## 🚀 Features
- Scrape jobs from LinkedIn (or other job boards)
- Extract structured attributes:
  - `title`
  - `company`
  - `location`
  - `description`
  - `applyLink`
  - `score`
  - `coverLetter`
- Append or update jobs in **Google Sheets**
- Prevent duplicate entries with **Upsert by jobid**
- Easy to customize and extend

---

## 📂 Workflow Overview
1. **Create Search URL** → generates LinkedIn (or board) search queries  
2. **Fetch Jobs** → pulls raw job postings  
3. **Extract Job Links** → isolates job URLs  
4. **Loop Over Items** → iterates through each posting  
5. **Fetch Job Page** → gets job HTML  
6. **Parse Job Attributes** → extracts structured data (title, company, location, description, applyLink, etc.)  
7. **Modify Job Attributes** → standardizes and adds `jobid`  
8. **Score Filter** → filters by `score >= 50`  
9. **Append/Upsert to Google Sheets** → stores results

---

## 🛠️ Setup Instructions
1. Install [n8n](https://docs.n8n.io/getting-started/installation/) locally or use n8n.cloud
2. Clone or import this workflow JSON into your n8n instance
3. Configure credentials:
   - **Google Sheets API** → connect your Google account
   - (Optional) **HTTP nodes** for LinkedIn/job board fetching
4. In Google Sheets:
   - Create a sheet with headers:
     ```
     jobid | title | company | location | score | coverLetter | description | link
     ```
   - Use `jobid` as the **Key Column** in the Google Sheets node
5. Run the workflow!

---

## ⚡ Troubleshooting
- **Nothing is being added** → Check that `jobid` exists in the input JSON and matches the Key column header in the Sheet.
- **Duplicate rows** → Make sure Upsert mode is enabled and `jobid` is unique.
- **Conversion error in Score Filter** → Wrap with `{{ Number($json.score) }}` to ensure numeric comparison.
- **Empty fields in sheet** → Verify JSON path (e.g., `{{ $('Modify Job Attributes').item.json.company }}`).

---

## 📌 Example Row in Google Sheets
| jobid  | title          | company                 | location            | score | coverLetter         | description         | link                  |
|--------|----------------|-------------------------|---------------------|-------|---------------------|---------------------|-----------------------|
| 12345  | Staff Engineer | Cavendish Professionals | Berlin, Deutschland | 87    | I am a great fit... | 🚀 We're Hiring ... | https://linkedin.com/... |

---

## 📜 License
MIT License – feel free to use and modify.
