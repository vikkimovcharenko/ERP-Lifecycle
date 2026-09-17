# End-to-End Procurement & Manufacturing Lifecycle

##  Project Overview
This repository contains business analysis documentation for a comprehensive Procure-to-Pay (P2P) and Manufacturing process. It demonstrates the flow of data and physical goods, starting from the initial need for materials to their utilization in production. The described processes and requirements are system-agnostic and applicable to modern enterprise ERP environments.

## Process Scope & Integrations
The project covers the following end-to-end business phases, including external system integrations:
1. **Purchase Requisition (PR):** Internal request for manufacturing parts (Part X).
2. **Vendor Management:** Vendor selection (RFQ) and onboarding/registration in the ERP system.
3. **Procurement & AP:** Purchase Order (PO) creation, Invoice receipt, and AP matching.
4. **Inventory & Quality Control:** Goods receipt, quality inspection, and stock placement.
5. **Payment Processing (API Integration):** Integration with a Bank / Payment Gateway to execute vendor payments based on approved invoices.
6. **Manufacturing:** Issuing Part X from inventory to the shop floor/production order.
7. **Data Warehouse Export (ETL):** Exporting financial (AP, Payments) and operational (Inventory, PO) data to a corporate Data Warehouse for BI reporting.

##  Documentation Structure
*(This section will be updated as the project progresses)*
- `01_Purchase_Requisition/` - Business requirements and process flow for PR.
- `...`
- `05_Payment_Integration/` - API specifications, JSON payloads, and sequence diagrams.
- `07_DWH_Export/` - Data mapping tables and ETL logic for the data warehouse.

## Tools & Techniques Used
- Business Process Modeling (BPMN) & Sequence Diagrams
- Requirements Engineering (User Stories, Acceptance Criteria)
- API Documentation & Data Mapping (Source-to-Target mapping)
