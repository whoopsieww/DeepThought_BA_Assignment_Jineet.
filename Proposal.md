# Operational Blueprint: 1,000-Company Scaled Sourcing Pipeline

This proposal outlines the strategic, operational, and mathematical workflows required to build, audit, and deliver a gold-standard master list of 1,000 ICP-qualified mid-market manufacturers within a 30-day window.

---

## ⚡ Concrete Filtering Criteria & Exclusion Logic

To ensure absolute data discipline, a company is only qualified if it satisfies all of our strict multi-gate criteria. The automated pipeline enforces these parameters by actively throwing out non-compliant data at each stage:

### 1. The Financial Scale Criteria (The Size Gate)
*   **The Parameters:** Annual Operational Revenue must sit strictly between ₹50 Crore and ₹500 Crore. Authorized and Paid-up Capital must be $\ge$ ₹5 Crore.
*   **How the System Filters It Out:** The database engine scans the core financial numbers filed with the MCA registry. If a company's revenue clocks in at ₹49 Crore or ₹501 Crore, the system automatically triggers an exclusion rule, drops the row from the active target queue, and routes it to a separate disqualification pipeline sheet with a logged reason code.

### 2. The Geographic Location Criteria (The Cluster Gate)
*   **The Parameters:** Plant operations must reside inside major Indian automotive and heavy engineering manufacturing hubs: Pune, Chennai, Ahmedabad, or Delhi-NCR.
*   **How the System Filters It Out:** The pipeline runs a localized text-matching search across address fields looking for industrial zone tags (like Chakan, Bhosari, Pimpri, Sanand, or Sriperumbudur). If a company is located outside these specific industrial manufacturing hubs, the system instantly flags it as an "out-of-cluster outlier" and drops it from the pipeline.

### 3. The Structural Business Model Criteria (The Moat Gate)
*   **The Parameters:** Must be a core asset-heavy manufacturer possessing a high technical entry barrier (precision engineering). No trading houses or low-moat commodity shops.
*   **How the System Filters It Out:** An AI scraping agent automatically crawls the company's website and scans their listed product catalogs. It runs a binary classification check: if the text matches keywords like "distributor," "trader," "wholesaler," or "basic metal fabricator," the system terminates processing for that company and instantly drops it. Only companies verified as making custom components (like gears, shafts, metrology, or seals) pass through.

---

## 📅 Detailed Weekly Sourcing & Filtering Timeline

### 🛠️ Week 1: Universe Ingestion & Automated Size/Geography Dropping
*   **Objective:** Ingest the broad national database and instantly eliminate scale and location outliers.
*   **Operations:** We scrape the broad Indian industrial database from the MCA registry based on National Industrial Classification (NIC) Codes 25 (Metal Products), 28 (Machinery), and 29 (Auto Components).
*   **Filtering Execution:** **This week, the pipeline completely executes the Size Gate and Geography Gate filters.** The system runs a programmatic check across all 15,000 raw rows, instantly throwing away any company that violates our ₹50Cr–₹500Cr revenue bracket or sits outside our target industrial cities.
*   **Expected Yield:** Reduces ~15,000 raw national rows down to **~6,000 cluster-aligned mid-market rows**.

### 🤖 Week 2: Asynchronous Scraping & AI Moat Filtering
*   **Objective:** Eliminate low-margin trading firms, commercial resellers, and basic commodity garages.
*   **Operations:** An automated web-crawler visits the active website URLs of the 6,000 surviving companies from Week 1 to pull down their product descriptions and factory facility text.
*   **Filtering Execution:** **This week, the pipeline completely executes the Moat Gate filter.** The system passes the scraped text through a semantic AI screening model. Any company classified by the AI as a low-moat trader or non-manufacturer is automatically dropped from the queue and moved to the fail list.
*   **Expected Yield:** Discards roughly 50% of remaining rows, leaving **~3,000 verified engineering targets**.

### 🔍 Week 3: Decision-Maker Matching & Quality Control Overrides
*   **Objective:** Extract leadership contact details and resolve boundary-line exceptions.
*   **Operations:** The pipeline queries corporate databases and professional networks to match surviving targets with their active Managing Directors, Promoters, or CEOs. 
*   **Filtering Execution:** **This week, the system filters out companies based on data completeness and manages edge cases.** If a target has absolutely zero reachable leadership data on file, it is dropped to protect database quality. Simultaneously, a data trap routes borderline revenue cases (e.g., an incredible aerospace part factory at ₹48 Crore) to a separate "Manual Audit" folder for a quick 60-second human verification pass.
*   **Expected Yield:** Cleans and refines the database down to **~1,250 completely enriched entries**.

### 🏁 Week 4: Final Validation Checks, Personalization & Master Sheet Export
*   **Objective:** Eradicate empty data cells, draft specific outreach copy, and compile the final deliverable.
*   **Operations:** The pipeline runs a final data-integrity scan across all tracking columns from Column A to AE. 
*   **Filtering Execution:** The system runs a strict validation sweep. If any critical data cell is blank, the row is flagged for immediate correction. The pipeline then selects the top 1,000 highest-conviction targets, uses a text generator to write a tailored 3-line outreach hook based on their shop-floor capabilities, and locks the final sheet.
*   **Final Output Yield:** Tab 1: `Master_Target_List` (**Exactly 1,000 high-conviction rows**), Tab 2: `Disqualified_Pipeline` (**~14,000 dropped rows** with clearly logged reasons for exclusion to prove complete filtering discipline).
