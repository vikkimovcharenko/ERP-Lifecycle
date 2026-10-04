# 03. System Roles & Permissions (Separation of Duties)

## 1. Business Context
To comply with audit standards (e.g., SOX) and ensure strict Separation of Duties (SoD), system access is compartmentalized by functional roles. Users can only access the modules and perform actions necessary for their specific job functions.

## 2. Role Definitions

| Functional Area | System Role | Key Permissions & Responsibilities | Restricted From |
| :--- | :--- | :--- | :--- |
| **Master Data** | **Item Admin** | Creates and manages Item Master data, BOMs, and catalogs. | Procurement Execution, Inventory |
| | **Compliance Officer** | Verifies vendor legal documents and activates Vendor records. | Creating POs, Making Payments |
| **Procurement** | **Procurement Manager** | Negotiates contracts, manages Non-Catalog POs, resolves Price Holds. | Approving Invoices, Receiving Goods |
| | **CC Responsible** | Approves PRs based on departmental budget (Cost Center). | Creating POs, Invoice Approval |
| **Logistics & QA** | **QC Manager** | Inspects deliveries, routes defects to Quarantine, creates RTVs/Credit Memos. | Approving POs, Financial Payments |
| | **Stock Keeper** | Scans barcodes for Goods Receipt; views only "Production Available" (PA) stock. | Viewing Quarantined Stock, Invoice Entry |
| **Manufacturing** | **Production Manager** | Manages Production Plan, initiates Work Orders, triggers Scrap for WIP. | Creating POs, QA Testing |
| | **QA Engineer** | Tests WIP/Finished Goods; approves batches or triggers Rework/Scrap. | Initiating Work Orders, Financial Write-offs |
| **Finance (AP)** | **AP Accountant** | Registers invoices, validates 3-Way Match. | Logistics, Goods Receipt |
| | **Financial Controller**| Approves high-value invoices, approves high-value Scrap write-offs. | Invoice Entry, Payment Execution |
| | **Payment Manager** | Authorizes bank API payments for approved invoice batches. | Invoice Registration, PO Creation |

## 3. Key User Stories
*   **US-ROLE-01:** As a System Administrator, I want to assign users to strict functional roles so that no single user can both create a Purchase Order and approve its corresponding Invoice (SoD).
