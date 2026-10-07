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
  * [PR Approval Matrix](./01_System_Setup/01_PR_Aprproval_Matrix.md)
  * [Invoice Approval Matrix](./01_System_Setup/02_Invoice_Approval_Matrix.md)
  * [System Roles & Permissions (SoD)](./01_System_Setup/03_System_Roles_and_Permissions.md)
* **02. Master Data Management (MDM)**
  * [Item creation)](./02_Master_Data_Management/01_Item_Creation.md)
  * [Vendor Onboarding](./02_Master_Data_Management/02_Vendor_Onboarding.md)
  * [Bill of Materials](./02_Master_Data_Management/03_Bill_of_Materials.md/)
  * [Customer Onboarding & Credit Management](./02_Master_Data_Management/04_Customer_Onboarding.md)

### Phase 2: Procure-to-Pay (P2P)
* **03. Procurement Execution**
  * [Purchase Requisition & MRP Triggers](./03_Procurement_Execution/01_Purchase_Requisition.md)
  * [Contract Managment](./03_Procurement_Execution/02_Contract_Managemegent.md)
  * [Purchase Order Generation](./03_Procurement_Execution/03_Purchase_Order.md)
* **04. Logistics & Quality Control**
  * [Goods Receipt and QC ](./04_Logistics_and_QC/01_Goods_Receipt_and_QC.md)
* **05. AP Financial Execution**
  * [AP Invoice Matching (3-Way Match) & Payments](./05_Financial_Execution_and_Payment/01_AP_Invoice_Matching.md)

### Phase 3: Build, Fulfill & Support
* **06. Manufacturing & QA**
  * [Production Execution, WIP & Scrap Routing](./06_Manufacturing/01_Production_Execution.md)
* **07. Order Fulfillment & Sales**
  * [Finished Goods Dispatch & Invoicing](./07_Order_Fulfillment_and_Sales/01_Finished_Goods_and_Dispatch.md)
  * [Customer Returns (RMA) & Traceability](./07_Order_Fulfillment_and_Sales/02_Customer_Returns_RMA.md)

---
**Author:** [Viktoriia Ovcharenko] - Business Analyst / Product Owner
