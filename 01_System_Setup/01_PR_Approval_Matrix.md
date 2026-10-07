# 01. System Setup & Procurement Approval Matrix

## 1. System Setup Prerequisites

### 1.1. Financial & Organizational Structure
*   **Entity & GL:** Entity setup completed; General Ledger (GL) structure defined.
*   **Accounts:** GL accounts uploaded to the system.
*   **Descriptive Flexfields (DFF):** DFFs configured for each segment of the GL account to capture flexible financial data; DFF data uploaded.
*   **Hierarchy:** Employee list uploaded with reporting hierarchies, financial information (GL accounts), and Cost Centers (CC).

### 1.2. Security & Role Setup
*   **Production Manager:** Granted permissions to create Purchase Requisitions (PR) and initiate Request to Create an Item.
*   **Item Admin:** Granted permissions to create/update items in the master catalog.
*   **Procurement Manager:** Granted permissions to create vendors, contracts, and Purchase Orders (PO).
*   **Approval Matrix:** System routing rules configured based on PR type (Catalogue/Non-Catalogue) and amount thresholds.

---

## 2. Delegation of Authority (Approval Matrix)

**Routing Rule:** All approvals are strictly **sequential**. A Requisition must be approved by the preceding authority level before it is routed to the next tier. The workflow stops routing once the final approver for the respective PR amount has signed off.

| Requisition Type | Amount Threshold (EUR) | Approver (Sequential Step) |
| :--- | :--- | :--- |
| **Catalogue PR** | €0 - €999.99 | Auto-Approval |
| **Catalogue PR** | ≥ €1,000.00 | Step 1: Production Director |
| **Catalogue PR** | > €50,000.00 | Step 2: CFO |
| **Non-Catalogue PR** | €0 - €999.99 | Step 1: Production Director |
| **Non-Catalogue PR** | ≥ €1,000.00 | Step 2: Cost Center (CC) Responsible Person |
| **Non-Catalogue PR** | ≥ €10,000.00 | Step 3: Procurement Manager* |
| **Non-Catalogue PR** | > €50,000.00 | Step 4: CFO |

*\*Note: If a Non-Catalogue PR contains a request for a new vendor, an additional parallel Sourcing process from the Procurement Manager is triggered upon their approval step.*

---

## 3. Sequential Workflow Diagram

```mermaid
flowchart TD
    Start((PR Submitted)) --> Type{PR Type?}
    
    %% Catalogue PR Workflow
    Type -- Catalogue --> CatAmount1{Amount >= €1,000?}
    CatAmount1 -- No --> Auto[Auto-Approved] --> End1(((Approved)))
    
    CatAmount1 -- Yes --> PD1[1. Production Director]
    PD1 --> PD1_Check{Approved?}
    PD1_Check -- No --> Rejected(((Rejected)))
    
    PD1_Check -- Yes --> CatAmount2{Amount > €50,000?}
    CatAmount2 -- No --> End1
    
    CatAmount2 -- Yes --> CFO1[2. CFO]
    CFO1 --> CFO1_Check{Approved?}
    CFO1_Check -- No --> Rejected
    CFO1_Check -- Yes --> End1

    %% Non-Catalogue PR Workflow
    Type -- Non-Catalogue --> PD2[1. Production Director]
    PD2 --> PD2_Check{Approved?}
    PD2_Check -- No --> Rejected
    
    PD2_Check -- Yes --> NonCatAmount1{Amount >= €1,000?}
    NonCatAmount1 -- No --> End2(((Approved)))
    
    NonCatAmount1 -- Yes --> CC[2. CC Responsible]
    CC --> CC_Check{Approved?}
    CC_Check -- No --> Rejected
    
    CC_Check -- Yes --> NonCatAmount2{Amount >= €10,000?}
    NonCatAmount2 -- No --> End2
    
    NonCatAmount2 -- Yes --> PM[3. Procurement Manager]
    PM --> PM_Check{Approved?}
    PM_Check -- No --> Rejected
    
    PM_Check -- Yes --> VendorCheck{New Vendor<br>Requested?}
    VendorCheck -- Yes --> Parallel[Trigger Parallel<br>Procurement Process] --> NonCatAmount3
    VendorCheck -- No --> NonCatAmount3{Amount > €50,000?}
    
    NonCatAmount3 -- No --> End2
    
    NonCatAmount3 -- Yes --> CFO2[4. CFO]
    CFO2 --> CFO2_Check{Approved?}
    CFO2_Check -- No --> Rejected
    CFO2_Check -- Yes --> End2

    classDef default fill:#f9f9f9,stroke:#333,stroke-width:1px;
    classDef decision fill:#e1f5fe,stroke:#03a9f4,stroke-width:2px;
    class Type,CatAmount1,CatAmount2,NonCatAmount1,NonCatAmount2,NonCatAmount3,PD1_Check,PD2_Check,CFO1_Check,CFO2_Check,CC_Check,PM_Check,VendorCheck decision;

```

---


## 4. Key User Stories

*   **US-SYS-01:** As a System Administrator, I want to configure Descriptive Flexfields (DFF) for GL accounts so that financial data can be captured flexibly according to the entity's structure.
*   **US-AM-01:** As the System, I want to enforce sequential routing so that higher-level executives (e.g., CFO) only review Requisitions that have already been vetted and approved by lower-tier management.
*   **US-AM-02:** As a Procurement Manager, I want to review any Non-Catalogue PR exceeding €10,000 after the CC Responsible has approved it, to ensure compliance before final CFO escalation.




    
