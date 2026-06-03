# Operational Blueprint: 1,000-Company Scaled Sourcing Pipeline

This proposal outlines the exact technical and operational workflows required to build, audit, and deliver a gold-standard master list of 1,000 ICP-qualified mid-market manufacturers within a 30-day window.

---

## 📅 Detailed Weekly Execution Plan

### 🛠️ Week 1: Universe Ingestion & Financial Filtering
*   **Objective:** Extract the broad universe of corporate entities sitting within target manufacturing sectors.
*   **Data Sources:** Ministry of Corporate Affairs (MCA) registries, Registrar of Companies (ROC) directories, and corporate database aggregators (Tofler, Zauba Corp, Probe42).
*   **Technical Actions:** 
    *   Write a Python data ingestion script utilizing `pandas` to query the aggregator APIs.
    *   Filter down the entire corporate index of India by pulling entries registered under **National Industrial Classification (NIC) Codes 25 (Fabricated Metals), 28 (Machinery & Equipment), and 29 (Motor Vehicles and Auto Components)**.
    *   Apply hard financial boundaries: **Paid-up Capital > ₹5 Crore** and **Annual Revenue between ₹50 Crore and ₹500 Crore**.
*   **Expected Yield:** ~15,000 raw unverified company rows.

### 🤖 Week 2: Automated AI Screening & Moat Validation
*   **Objective:** Eliminate low-margin trading firms and commodity job shops at scale without manual reading.
*   **Technical Actions:**
    *   Deploy an asynchronous Python web-scraping script running `aiohttp` and `BeautifulSoup` to crawl the active homepages and product directories of the 15,000 URLs collected in Week 1.
    *   Pipe the extracted raw text blocks into the **Gemini 1.5 Pro / Claude 3.5 Sonnet API** using a strict JSON-returning prompt instruction:
        *   *Is this entity a physical manufacturer or a trading house?*
        *   *Does the company produce high-fatigue, custom-engineered products (e.g., planetary gearboxes, sub-micron probes, mechanical seals) or generic commodity items?*
    *   Parse the AI response programmatically to automatically drop companies flagged as traders or resellers.
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
    *   Feed the company's verified product niche into an automated text generation prompt to craft a **3-line Personalization Hook** for every company (e.g., *"I was tracking your advanced CNC wire-winding setups in Pimpri and was highly impressed by your custom flexible shaft capabilities..."*).
    *   Compile and export the final spreadsheet into a professional workbook with two clear tabs: `Master_Target_List` (Exactly 1,000 highly qualified rows) and `Disqualified_Pipeline` (~14,000 rows with clear dropout reasons logged to prove absolute data discipline).

---

## 📊 Expected Sourcing Funnel Yield
*   **Initial Database Pull:** ~15,000 companies (Raw NIC & Revenue match)
*   **Post-AI Moat Screen:** ~3,000 companies (Traders & commodity shops removed)
*   **Post-Data Enrichment & Audit:** ~1,250 companies (Decision makers found, edge cases resolved)
*   **Final Export List:** **1,000 Gold-Standard Targets**