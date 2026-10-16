# 08. Reporting, Analytics & BI Data Architecture

## 1. Requirements Engineering & BI Governance Framework

### 1.1 Business Requirements Overview
The Reporting & Analytics layer provides operational visibility and executive decision-making capabilities across all ERP modules. To support day-to-day operations while enabling strategic analytics, the system separates real-time transactional reporting (OLTP) from aggregated business intelligence (OLAP).

### 1.2 Non-Functional Requirements (NFRs)
*   **NFR-BI-01 (Data Latency):** Operational dashboards must refresh within **<= 15 minutes** of transactional commit.
*   **NFR-BI-03 (Row-Level Security - RLS):** Report visibility is dynamically filtered by the user's role and Cost Centre.
*   **NFR-BI-04 (Export Compliance):** All reports must support secure export to `.CSV` and `.XLSX` with an execution timestamp and user ID.

---

## 2. Technical Report Specifications

### Specification 1: Real-Time Operational QA & Work Order Testing Summary (Standard Production)
**Objective:** Track daily production quality, scrap losses, and rework efficiency for standard goods.

| Column # | Field Name | Data Source / Filter Condition | Mathematical Formula |
| :--- | :--- | :--- | :--- |
| **Col 1** | **Pending QA Handover** | `Work_Orders` where `Routing_Stage = Stage 2` AND `WO_Status = Completed` AND `QA_Status = Pending` | $\sum \text{WO Quantity}$ |
| **Col 2** | **Total In QA Testing** | `Work_Orders` where `WO_Status = In Testing` | $\sum \text{WO Quantity}$ |
| **Col 3.1** | **QA Passed** | `Work_Orders` where `QA_Status = Passed` AND `FG_Receipt_Status = Pending` | $\sum \text{Passed WO Quantity}$ |
| **Col 3.2** | **Rework Triggered** | `Rework_Orders` linked to `Work_Orders` tested in period | $\sum \text{Reworked Qty}$ |
| **Col 3.3** | **Scrapped in QA** | `Scrap_Documents` linked to `Work_Orders` tested in period | $\sum \text{Scrapped Qty}$ |
| **Col 4.1** | **% Pass** | Subcol 3.1 / Col 2 | $(\text{Col 3.1} / \text{Col 2}) \times 100\%$ |
| **Col 4.2** | **% Rework Rate** | Subcol 3.2 / Col 2 | $(\text{Col 3.2} / \text{Col 2}) \times 100\%$ |
| **Col 4.3** | **% QA Scrap Rate** | Subcol 3.3 / Col 2 | $(\text{Col 3.3} / \text{Col 2}) \times 100\%$ |

*Validation Rule:* Total Absolute Count (Col 3.1 + 3.2 + 3.3) + In-Progress QA Inventions = Count of Records in `QA_Inspection_Header`.
*Visualization:* 100% Stacked Bar Chart tracking % Passed, % Reworked, and % Scrapped over time.

---

### Specification 2: Experimental / R&D Testing Outcomes
**Objective:** Track pass/fail rates, categorize defect reasons via structured DFF (Descriptive Flexfields), and measure financial burn rate for R&D batches.

| Column # | Field Name | Data Source / Filter Condition | Mathematical Formula |
| :--- | :--- | :--- | :--- |
| **Col 1** | **Total Ready for 2nd QA** | `Experimental_Batches` where `Status = Transferred to R&D` | $\sum \text{Unit Qty}$ |
| **Col 2.1** | **Passed (Absolute Qty)** | `Experimental_Batches` where `Status = Passed` | $\sum \text{Passed Qty}$ |
| **Col 2.2** | **Passed (%)** | Col 2.1 / Col 1 | $(\text{Col 2.1} / \text{Col 1}) \times 100\%$ |
| **Col 3.1** | **Fail: Reason 1 (%)** | `Scrap_Docs` where `Reason_Code = [Code 1]` | $(\sum \text{Reason 1 Qty} / \text{Col 1}) \times 100\%$ |
| **Col 3.2** | **Fail: Reason 2 (%)** | `Scrap_Docs` where `Reason_Code = [Code 2]` | $(\sum \text{Reason 2 Qty} / \text{Col 1}) \times 100\%$ |
| **Col 4.1** | **Total R&D Value (Cost)**| Standard cost of Col 1 | $\text{Col 1 Qty} \times \text{Standard Unit Cost}$ |
| **Col 4.2** | **Passed Value (Cost)** | Standard cost of Col 2.1 | $\text{Col 2.1 Qty} \times \text{Standard Unit Cost}$ |
| **Col 4.4** | **Scrap Value (Cost)** | Standard cost of all failed units | $\sum (\text{Failed Qty} \times \text{Standard Unit Cost})$ |

*Visualization:* Clustered Bar Chart tracking Total R&D Cost vs. Passed Value vs. Scrapped Value (in Local Currency) per Product.
