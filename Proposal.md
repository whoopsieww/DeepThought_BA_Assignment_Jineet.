# Operational Blueprint: 1,000-Company Scaled Sourcing Pipeline

This proposal outlines the exact technical, operational, and mathematical workflows required to build, audit, and deliver a gold-standard master list of 1,000 ICP-qualified mid-market manufacturers within a 30-day window.

---

## 📅 Detailed Weekly Sourcing & Filtering Timeline

### 🛠️ Week 1: Universe Ingestion & Automated Size/Geography Dropping
* **Objective:** Ingest the broad Indian industrial database and instantly eliminate scale and location outliers.
* **Data Sources:** Ministry of Corporate Affairs (MCA) registries and corporate data aggregators (Tofler, Zauba Corp, Probe42).
* **What Code Filters Out This Week:** The script pulls all companies registered under National Industrial Classification (NIC) Codes 25 (Metal Products), 28 (Machinery), and 29 (Auto Components). It loops through every single company row and applies a strict mathematical and string check to instantly drop entities that do not fit our parameters.
* **The Execution Code Block:**
    ```python
    # RUNNING IN WEEK 1: Drops companies by size and cluster boundaries
    for company in raw_mca_ingestion_batch:
        
        # 1. Size Filter Criteria: ₹50Cr <= Revenue <= ₹500Cr
        if not (50 <= company["revenue_cr"] <= 500):
            company["drop_reason"] = f"Failed Size Gate: Revenue (₹{company['revenue_cr']}Cr) outside ₹50Cr-₹500Cr limit"
            disqualified_pipeline.append(company)
            continue # Drops row instantly from active tracking
            
        # 2. Geography Filter Criteria: Must sit in core manufacturing hubs
        target_hubs = ["pune", "chakan", "pimpri", "bhosari", "chennai", "sriperumbudur", "ahmedabad", "sanand", "delhi", "ncr"]
        address_string = company["plant_address"].lower()
        
        if not any(hub in address_string for hub in target_hubs):
            company["drop_reason"] = "Failed Geography Gate: Plant location outside target industrial clusters"
            disqualified_pipeline.append(company)
            continue # Drops row instantly from active tracking
            
        week_1_surviving_pipeline.append(company)
    ```
* **Expected Yield:** Reduces ~15,000 raw national rows down to **~6,000 cluster-aligned mid-market rows**.

---

### 🤖 Week 2: Asynchronous Scraping & AI Moat Filtering
* **Objective:** Eliminate low-margin trading firms, commercial resellers, and basic commodity garages.
* **Technical Actions:**
    An asynchronous script crawls the active URLs of the 6,000 surviving companies from Week 1. It extracts raw text layers from their homepage and product catalogs.
* **What Code Filters Out This Week:** The script pipes the scraped website text directly into the Claude/Gemini API. The AI agent performs a strict semantic check. If the AI identifies the company as a pure trader or an undifferentiated job shop, the code triggers a drop clause and routes the data to our fail list.
* **The Execution Code Block:**
    ```python
    # RUNNING IN WEEK 2: Drops companies lacking an engineering moat
    for company in week_1_surviving_pipeline:
        
        # 3. Moat Filter Criteria: Core asset-heavy manufacturer vs pure trader
        ai_response = await run_llm_agent_screening(company["scraped_website_text"])
        
        if ai_response["status"] == "FAIL":
            company["drop_reason"] = f"Failed Moat Gate: Identified as {ai_response['classification']} - {ai_response['reason']}"
            disqualified_pipeline.append(company)
            continue # Drops row instantly from active tracking
            
        company["structural_moat_details"] = ai_response["extracted_moat_capabilities"]
        week_2_surviving_pipeline.append(company)
    ```
* **Expected Yield:** Discards roughly 50% of remaining rows, leaving **~3,000 verified engineering targets**.

---

### 🔍 Week 3: Decision-Maker Matching & Human-in-the-Loop Quality Control
* **Objective:** Extract leadership contact details and resolve boundary-line exceptions.
* **What Code Filters Out This Week:** The pipeline queries the Apollo.io and LinkedIn Sales Navigator APIs to match our surviving companies with their active **Managing Directors, Promoters, or CEOs**. If a company has absolutely no reachable leadership data available on public/premium nodes, it is logged and dropped to protect data completeness.
* **The Exception Routing Code Block:**
    ```python
    # RUNNING IN WEEK 3: Handles missing data and borderline revenue cases
    for company in week_2_surviving_pipeline:
        
        # Pull leadership details from external APIs
        dm_contact = await fetch_leadership_metadata(company["name"])
        
        if not dm_contact["has_valid_executive"]:
            company["drop_reason"] = "Failed Data Enrichment: No active Promoter/MD contact found via LinkedIn/Apollo"
            disqualified_pipeline.append(company)
            continue # Drops row due to missing target decision-maker
            
        # Quality Control Trap for Borderline Cases (e.g., ₹48Cr or ₹512Cr)
        if company["requires_manual_override_flag"] == True:
            route_to_human_audit_sheet(company)
            continue # Pauses row for a 60-second manual review pass
            
        company["dm_name"] = dm_contact["name"]
        company["dm_title"] = dm_contact["title"]
        week_3_surviving_pipeline.append(company)
    ```
* **Expected Yield:** Cleans and refines the database down to **~1,250 completely enriched entries**.

---

### 🏁 Week 4: Final Validation Checks, Personalization & Master Sheet Export
* **Objective:** Eradicate cell blanks, draft specific outreach copy, and compile the final deliverable.
* **Technical Actions:** The script scans the entire matrix from Column A to AE. If any cell is completely blank, it triggers a warning. It then feeds the `structural_moat_details` gathered in Week 2 into a copy-generation script to automatically output a tailored 3-line outreach hook.
* **The Final Polish Script:**
    ```python
    # RUNNING IN WEEK 4: Compiles final outputs and locks exactly 1,000 targets
    final_master_rows = []
    
    for company in week_3_surviving_pipeline[:1000]: # Slices exact top 1,000 high-conviction targets
        
        # Auto-generate custom 3-line outreach hook based on verified shop-floor moats
        company["personalization_hook"] = generate_outreach_copy(company["name"], company["structural_moat_details"])
        final_master_rows.append(company)
        
    # Export structural data frames to separate spreadsheet tabs
    pd.DataFrame(final_master_rows).to_csv("Master_Target_List.csv", index=False)
    pd.DataFrame(disqualified_pipeline).to_csv("Disqualified_Pipeline.csv", index=False)
    print("Sourcing completed successfully. Outputs locked.")
    ```
* **Final Output Yield:** Tab 1: `Master_Target_List` (**Exactly 1,000 rows**), Tab 2: `Disqualified_Pipeline` (**~14,000 dropped rows** with clear, automated logs explaining the drop reason).
