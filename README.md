# Timmermann Lab 

# 📄 8-K Guidance Sentiment Extraction Pipeline

This project processes 8-K and 8-K/A filings, identifies "guidance"-related disclosures, links them to firm identifiers, calculates sentiment scores, and generates a structured dataset for financial research.

---

## ✅ Task Breakdown

### 1. Filter for 8-K and 8-K/A Filings Only

- From the raw dataset, isolate and **copy/move** only:
  - `8-K` filings
  - `8-K/A` filings (where "A" = amended)
- Save these files into the `extracted_8k/` folder.

---

### 2. Extract CIK Code and Link to Firm Identifiers

- Extract the **CIK code** from each 8-K/8-K/A filing.
- Match each CIK **and year** using the provided translation files to retrieve:
  - **Firm Name**
  - **CUSIP**
  - **GVKEY**
- Translation files are located in the `cik_translation/` directory.

---

### 3. Identify "Guidance" Paragraphs

- From each filing in `extracted_8k/`, parse and extract all paragraphs or sentences that mention or discuss:
  - `"guidance"` (case-insensitive search)
- These extracted sections will be used for sentiment analysis.

---

### 4. Generate Sentiment Scores on Guidance Sections

- For each extracted "guidance" section, compute a **sentiment score between 0 and 1**:
  - `0 = extremely negative`
  - `1 = extremely positive`
- If the entire 8-K is focused on guidance, you may use the WRDS pre-computed sentiment score as a proxy.
- Sample WRDS file:
  [SEC Text Sentiment CSV (Dropbox)](https://www.dropbox.com/scl/fi/toylyaj3rfjkgkt57b3w1/SECAnalyticsSuite_SECTextAnalysis_ReadabilityAndSentiment.csv?rlkey=0cl9diahscyn71jxuok2bvxl5&dl=0)

---

### 5. Construct Final Output Database

Each row in the final output CSV should include:

| FIRM CIK | FIRM NAME | CUSIP | GVKEY | FILING DATE | SENTIMENT SCORE |
|----------|-----------|--------|--------|--------------|------------------|

- Save the final CSV file in the `sentiment_outputs/` folder.

---

## 🛠 Dependencies

- Python 3.8+
- `pandas`, `numpy`, `re`, `nltk` or `spacy`
- `tqdm`
- `beautifulsoup4` or `lxml` (for parsing HTML filings)

---

## 💬 Notes

- Sentiment scoring method can be adjusted based on project needs.
- Ensure firm mapping is **year-specific** for accurate joins.
- Filings without guidance content should not receive a sentiment score.
