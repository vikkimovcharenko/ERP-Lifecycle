# Enterprise ERP Architecture Project

Welcome to my Business Analysis portfolio project. This repository contains the complete architectural design and business requirements for an Enterprise ERP ecosystem, covering Procure-to-Pay (P2P), Manufacturing, and Order Fulfillment.

## Project Scope & Core Principles
This project demonstrates a robust, enterprise-grade system design emphasizing:
- **Strict Separation of Duties (SoD)** for SOX and financial audit compliance.
- **End-to-End Lot/Batch Traceability** from raw materials to customer returns.
- **Automated MRP** (Material Requirements Planning) to prevent production downtime.
- **IFRS 15 Revenue Recognition** standards via Goods Issue-triggered invoicing.

**For a complete overview of the business logic, architecture, and system rules, please start here:** 
[**00_Master_BRD.md (Business Requirements Document)**](./00_Master_BRD.md)

---

## Table of Contents (System Modules)

### Phase 1: Foundation & Master Data
* **01. System Setup**
  * [PR Approval Matrix](./01_System_Setup/101_PR_Approval_Matrix.md)
  * [Invoice Approval Matrix](./01_System_Setup/102_Invoice_Approval_Matrix.md)
  * [System Roles & Permissions (SoD)](./01_System_Setup/103_System_Roles_and_Permissions.md)
* **02. Master Data Management (MDM)**
  * [Item creation](./02_Master_Data_Management/201_Item_Creation.md)
  * [Vendor Onboarding](./02_Master_Data_Management/202_Vendor_Onboarding.md)
  * [Bill of Materials](./02_Master_Data_Management/203_Bill_of_Materials.md/)
  * [Customer Onboarding & Credit Management](./02_Master_Data_Management/204_Customer_Onboarding.md)

### Phase 2: Procure-to-Pay (P2P)
* **03. Procurement Execution**
  * [Purchase Requisition & MRP Triggers](./03_Procurement_Execution/301_Purchase_Requisition.md)
  * [Contract Managment](./03_Procurement_Execution/302_Contract_Managemegent.md)
  * [Purchase Order Generation](./03_Procurement_Execution/303_Purchase_Order.md)
* **04. Logistics & Quality Control**
  * [Goods Receipt and QC ](./04_Logistics_and_QC/401_Goods_Receipt_and_QC.md)
* **05. AP Financial Execution**
  * [AP Invoice Matching (3-Way Match) & Payments](./05_Financial_Execution_and_Payment/501_AP_Invoice_Matching.md)

### Phase 3: Build, Fulfill & Support
* **06. Manufacturing & QA**
  * [Production Execution, WIP & Scrap Routing](./06_Manufacturing/601_Production_Execution.md)
* **07. Order Fulfillment & Sales**
  * [Finished Goods Dispatch & Invoicing](./07_Order_Fulfillment_and_Sales/701_Finished_Goods_and_Dispatch.md)
  * [Customer Returns (RMA) & Traceability](./07_Order_Fulfillment_and_Sales/702_Customer_Returns_RMA.md)

### Phase 4: Analytics & Intelligence
* **08. Reporting & Analytics**
  * [Reporting & BI Architecture](./08_Reporting_and_Analytics/801_Reporting_and_BI_Architecture.md)

### Phase 5: System Integrations
* **09. System Integrations & Interfaces**
  * [Financial Integrations: Outbound AP Payments](./9_System_Integrations/901_Financial_Integrations.md)
    
---
**Author:** [Viktoriia Ovcharenko] - Business Analyst / Product Owner
