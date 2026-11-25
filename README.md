# Third-Party Lifecycle Management (TLM) & Vendor Risk Tracker

## Project Overview
Financial institutions rely on third-party vendors for cloud hosting, payment processing, customer support, and data analytics—creating operational, cybersecurity, regulatory, and fourth-party concentration risks. This workbook (`TLM_Vendor_Risk_Tracker.xlsx`) implements an end-to-end **Third-Party Lifecycle Management (TLM)** governance framework aligned with banking risk standards (such as Federal Reserve SR 23-4 guidance).

The tracker automates inherent risk tiering, multi-pillar control scoring, residual risk classification, SLA breach monitoring, and executive Key Risk Indicator (KRI) reporting across a simulated portfolio of 10 banking vendors.

---

## TLM Lifecycle Methodology
The workbook models four sequential stages of third-party risk governance:

1. **Inherent Risk Scoping & Tiering (`Vendor_Inventory`)**: Evaluates each vendor's baseline risk exposure prior to any internal controls based on data sensitivity, business criticality, and fourth-party sub-processor reliance.
2. **Due Diligence & Control Assessment (`Due_Diligence`)**: Assesses control effectiveness on a 1–5 maturity scale across five banking risk pillars to determine **Residual Risk**.
3. **Issue & Remediation Governance (`Remediation_Log`)**: Logs control gaps discovered during due diligence, assigns severity ratings, and tracks remediation deadlines against Service Level Agreements (SLAs).
4. **Executive KRI Monitoring (`KRI_Dashboard`)**: Aggregates portfolio-level risk metrics and visual summaries to support senior leadership oversight and regulatory readiness.

---

## Workbook Architecture & Scoring Logic

### 1. `Vendor_Inventory` — Inherent Risk Tiering
Evaluates 10 vendors (`VND-001` through `VND-010`) across three risk drivers using nested `IF` logic:

* **Data Access Sensitivity**:
  * `PCI-DSS / PII`: **6 points**
  * `PII Only`: **4 points**
  * `Confidential`: **2 points**
  * `None`: **1 point**
* **Operational Criticality**:
  * `Critical`: **6 points**
  * `High`: **4 points**
  * `Medium`: **2 points**
  * `Low`: **1 point**
* **Fourth-Party Sub-Processor Reliance**:
  * `Yes`: **+3 points** | `No`: **0 points**

**Inherent Risk Tier Thresholds (Max Score = 15):**
* **Tier 1 – Critical**: Score `>= 12`
* **Tier 2 – High**: Score `8 to 11`
* **Tier 3 – Medium**: Score `5 to 7`
* **Tier 4 – Low**: Score `< 5`

---

### 2. `Due_Diligence` — 5-Pillar Control Scoring & Residual Risk
Links dynamically to `Vendor_Inventory` via `INDEX-MATCH` and scores assessed vendors from **1 (Weak/Missing)** to **5 (Strong/Verified)** across five core risk pillars:
1. **Information Security (`InfoSec`)**
2. **Business Continuity & Disaster Recovery (`BCP_DR`)**
3. **Data Privacy (`Privacy`)**
4. **Financial Health & Viability (`Financial`)**
5. **Fourth-Party Risk Governance (`4th_Party`)**

**Residual Risk Calculation Logic:**
* **Low Residual Risk**: Average Control Score `>= 4.2`
* **High Residual Risk**: Any **Tier 1 – Critical** vendor with Average Control Score `< 4.2` (strict banking escalation rule), OR any vendor with Average Control Score `< 3.0`
* **Medium Residual Risk**: Non-Tier 1 vendors with Average Control Score between `3.0` and `4.19`

---

### 3. `Remediation_Log` — Issue & SLA Breach Tracking
Tracks 5 control findings (`ISS-101` through `ISS-105`) and dynamically calculates SLA compliance using Excel's `TODAY()` function:
* Returns `"Closed on Time"` when `Status = "Closed"`.
* Flags `"OVERDUE (X days)"` whenever an open finding passes its `SLA_Due` date.
* Displays `"On Track"` for open or in-progress items still within their remediation window.

---

### 4. `KRI_Dashboard` — Executive Summary & Visuals
Consolidates live portfolio metrics using `COUNTA`, `COUNTIF`, and `COUNTIFS` formulas alongside conditional formatting and two executive charts:
* **Clustered Column Chart**: Distribution of Vendors by Inherent Risk Tier (`Tier 1 - Critical`: 3, `Tier 2 - High`: 2, `Tier 3 - Medium`: 3, `Tier 4 - Low`: 2)
* **Doughnut Chart**: Vendor Due Diligence Assessment Status Breakdown

---

## Key Risk Indicator (KRI) Insights from Portfolio Data

* **100% Tier-1 Assessment Completion**: All 3 `Tier 1 – Critical` vendors (`CloudVault Corp`, `SwiftPay Gateway`, and `FinMetrics Analytics`) have completed due diligence reviews, ensuring zero unassessed critical exposure.
* **Tier-1 Control Divergence**: While `CloudVault Corp` and `FinMetrics Analytics` achieved strong control averages (`4.6 / 5.0`, reducing residual risk to **Low**), `SwiftPay Gateway` scored `3.8 / 5.0` due to an expired SOC 2 Type II report—keeping its residual risk **High** despite Tier-1 criticality.
* **Concentrated High Residual Risk (3 of 7 Assessed Vendors)**: `SwiftPay Gateway` (`VND-002`), `PulseSend Marketing` (`VND-005`), and `ClearView Print` (`VND-007`) carry **High** residual risk driven by missing fourth-party encryption proof and untested disaster recovery plans.
* **SLA Escalation Priorities**: Out of 4 open remediation items, **3 findings are overdue** (`ISS-101`, `ISS-102`, and `ISS-103`), requiring immediate vendor governance escalation—led by `ClearView Print` (`VND-007`), which has both an overdue annual assessment and a critical overdue BCP/DR finding.

---

## Excel Skills & Functions Demonstrated
* **Dynamic Lookup & Logic**: `INDEX-MATCH`, nested `IF`, `AND`, `AVERAGE`
* **KPI & KRI Aggregation**: `COUNTA`, `COUNTIF`, `COUNTIFS`
* **Date-Driven Governance**: `TODAY()` dynamic SLA aging calculations
* **Visual Reporting**: Rule-based Conditional Formatting, Clustered Column Charts, and Doughnut Charts

---

## How to Use This Workbook
1. Download `TLM_Vendor_Risk_Tracker.xlsx` and open it in Microsoft Excel or Google Sheets.
2. Add new vendors in `Vendor_Inventory` (Columns A–F & I) and drag down Columns G–H to auto-assign Inherent Risk Tiers.
3. Log pillar scores (1–5) in `Due_Diligence` and open control findings in `Remediation_Log`; `KRI_Dashboard` updates automatically.
