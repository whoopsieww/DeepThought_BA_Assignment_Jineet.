# Operational Blueprint: 1,000-Company Scaled Sourcing Pipeline

This proposal outlines the exact technical, operational, and mathematical workflows required to build, audit, and deliver a gold-standard master list of 1,000 ICP-qualified mid-market manufacturers within a 30-day window.

---

## ⚡ The 3-Gate Verification & Filtering Criteria

To ensure the final dataset contains 1,000 genuine, high-conviction targets, the automated pipeline applies three strict operational, location, and financial gates. A company must clear all three gates to remain on the list.

### 1. The Size Gate (Financial Boundaries)
*   **The Criteria:** Annual Operational Revenue must sit strictly between ₹50 Crore and ₹500 Crore.
*   **How We Filter It:** We run a python filter on the revenue column extracted from the MCA registry data:
    $$\text{₹50 Crore} \le \text{Operating Revenue} \le \text{₹500 Crore}$$
*   **Why It Works:** This mathematical condition automatically filters out massive corporate giants (like Kalyani Technoforge at ₹2,520Cr) that are handled by global investment banks, while simultaneously dropping small, low-tech local job shops earning under ₹50 Crore.

### 2. The Geography Gate (Industrial Cluster Mapping)
*   **The Criteria:** Companies must be physically operating inside high-density manufacturing clusters (e.g., Pune, Chennai, Ahmedabad, Delhi-NCR).
*   **How We Filter It:** The pipeline runs a string-matching pattern search on the registered plant location/address columns to verify matching pin codes or industrial zones (like Chakan, Pimpri, Sanand, or Sriperumbudur).
*   **Why It Works:** It locks our data to areas that benefit from shared logistics routes, Tier-1 OEM ecosystems, and specialized technical labor, making their operating structures highly comparable.

### 3. The Moat Gate (Structural Business Model Check)
*   **The Criteria:** Must be a core asset-heavy manufacturer possessing a high technical entry barrier (precision engineering).
*   **How We Filter It:** We feed the website text collected by our scraper into the Claude/Gemini API to perform a semantic analysis based on a strict prompt:
    *   *Pure Traders/Distributors/Resellers* ➡️ **REJECT (Flagged as Low-Moat)**
    *   *Custom-Engineered Precision Part Makers (Gears, Springs, Seals, Metrology)* ➡️ **PASS**
*   **Why It Works:** This is our absolute quality gate. It instantly drops generic commercial trading firms and low-tech fabrication garages, ensuring 100% of the targets hold true pricing power.

---

## 📅 Detailed Weekly Execution Plan

### 🛠️ Week 1: Universe Ingestion & Financial Filtering (Gate 1 & 2)
*   **Objective:** Extract the broad universe of corporate entities sitting within target manufacturing sectors.
*   **Data Sources:** Ministry of Corporate Affairs (MCA) registries, Registrar of Companies (ROC) directories, and corporate data aggregators (Tofler, Zauba Corp, Probe42).
*   **Technical Actions:** 
    *   Write a Python data ingestion script utilizing `pandas` to query aggregator APIs.
    *   Filter down the entire corporate index of India by pulling entries registered under **National Industrial Classification (NIC) Codes 25 (Fabricated Metals), 28 (Machinery & Equipment), and 29 (Motor Vehicles and Auto Components)**.
    *   Apply the **Size Gate** and **Geography Gate** conditions immediately to drop outliers.
*   **Expected Yield:** ~15,000 raw company rows matching financial/cluster parameters.

### 🤖 Week 2: Automated AI Screening & Moat Validation (Gate 3)
*   **Objective:** Eliminate low-margin trading firms and commodity job shops at scale without manual reading.
*   **Technical Actions:**
    *   Deploy an asynchronous Python web-scraping script running `aiohttp` and `BeautifulSoup` to crawl the active homepages and product directories of the 15,000 URLs collected in Week 1.
    *   Pipe the extracted raw text blocks into the **Gemini 1.5 Pro / Claude 3.5 Sonnet API** using a strict JSON-returning prompt instruction to analyze the company's manufacturing capability and engineering moat.
    *   Parse the AI response programmatically to automatically drop companies flagged as low-moat traders or resellers.
*   **Expected Yield:** ~3,000 semi-qualified manufacturing targets.

### 🔍 Week 3: Decision-Maker Extraction & Human-in-the-Loop Quality Control
*   **Objective:** Locate key promoters, resolve data gaps, and handle borderline financial cases.
*   **Technical Actions:**
    *   For the 3,000 surviving targets, run automated scripts against **Apollo.io and LinkedIn Sales Navigator APIs** to extract the exact names and active email addresses of key leadership figures (**Managing Directors, Promoters, and Chief Executive Officers**).
    *   **Quality Control Routing:** Build a rule-based exception folder. If a company's revenue is marginally outside the boundary (e.g., ₹48 Crore or ₹515 Crore) but the AI notes an exceptional technical asset moat (like specialized defense or aerospace tooling), route it to a "Manual Audit" sheet for a quick, 60-second human verification pass.
*   **Expected Yield:** ~1,250 thoroughly verified, high-conviction targets.

### 🏁 Week 4: Data Polish, Automated Copywriting & Master Export
*   **Objective:** Format the final dataset, eliminate empty fields, and generate tailored outreach copy.
*   **Technical Actions:**
    *   Run a final validation script checking for any missing data elements in columns A through AE.
    *   Feed the company's verified product niche into an automated text generation prompt to craft a unique **3-line Personalization Hook** for every company.
    *   Compile and export the final spreadsheet into a professional workbook with two clear tabs: `Master_Target_List` (Exactly 1,000 highly qualified rows) and `Disqualified_Pipeline` (~14,000 rows with clear dropout reasons logged to prove absolute data discipline).

---

## 📊 Expected Sourcing Funnel Yield
*   **Initial Database Pull:** ~15,000 companies (Raw NIC, Size & Geography lock)
*   **Post-AI Moat Screen:** ~3,000 companies (Traders & commodity shops removed)
*   **Post-Data Enrichment & Audit:** ~1,250 companies (Decision makers found, edge cases resolved)
*   **Final Export List:** **1,000 Gold-Standard Targets**
