# Operational Blueprint: 1,000-Company Scaled Sourcing Pipeline

This proposal outlines the exact technical, operational, and mathematical workflows required to build, audit, and deliver a gold-standard master list of 1,000 ICP-qualified mid-market manufacturers within a 30-day window.

---

## ⚡ Concrete Filtering Criteria & Data Schema (Columns A–AE)

To ensure absolute data discipline, a company is only qualified if it satisfies all of our strict multi-gate criteria. Every company row in the master tracker must hold complete data points across these core variables:

### 1. The Financial Scale Criteria (The Size Gate)
*   **Operating Revenue Boundary:** Annual turnover must sit strictly between ₹50 Crore and ₹500 Crore ($\text{₹}50\text{Cr} \le \text{Revenue} \le \text{₹}500\text{Cr}$). 
*   **Capitalization Floor:** Authorized and Paid-up Capital must be greater than or equal to ₹5 Crore ($\text{Paid-up Capital} \ge \text{₹}5\text{Cr}$).
*   **Execution Logic:** Code filters automatically drop small job shops with erratic working capital cycles under ₹50Cr, alongside heavily institutionalized entities above ₹500Cr.

### 2. The Geographic Location Criteria (The Cluster Gate)
*   **Target Clusters:** Plant operations must reside inside major Indian automotive and heavy engineering manufacturing hubs: Pune MIDC (Chakan, Bhosari, Pimpri), Chennai (Sriperumbudur, Oragadam), Ahmedabad (Sanand, Changodar), or Delhi-NCR (Manesar, Faridabad).
*   **Execution Logic:** String-pattern matching filters look for localized pin codes and address strings. This ensures we leverage cluster economics—targeting companies that share logistics networks, raw material alloy suppliers, and specialized technical labor.

### 3. The Structural Business Model Criteria (The Moat Gate)
*   **Asset Footprint (C1 Metric):** Must feature heavy, physical operational assets (e.g., CNC machining centers, high-tonnage forging lines, heat treatment blocks).
*   **Differentiation Layer (C3 Metric):** Must manufacture custom-engineered precision items (e.g., planetary gearboxes, sub-micron metrology probes, mechanical seals, high-fatigue springs) rather than basic commodities.
*   **Execution Logic:** The web-scraping script extracts text layers from company product pages and passes them to the Claude/Gemini API to execute a binary check:
    *   *Pure Trading / Reselling / Basic Fabrication Garages* ➡️ **REJECT (Flagged as Low-Moat)**
    *   *Custom Engineering / Specialized Tooling / B2B OEM Tier-1 Supply* ➡️ **PASS**

---

## 📅 Detailed Weekly Execution Plan

### 🛠️ Week 1: Universe Ingestion & Financial Filtering (Gates 1 & 2)
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
