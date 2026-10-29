# 702. Customer Returns (RMA) & Traceability Resolution

## 1. Business Context
When a customer returns Finished Goods (FG), the Return Merchandise Authorization (RMA) process manages inbound logistics, financial resolution, and root-cause analysis. Leveraging End-to-End Lot/Batch Traceability, the system categorises the defect origin. Rather than applying instantaneous rating penalties, vendor-related defects are logged into the Historical Data Schema for processing by the nightly Batch Calculation Engine, preserving system performance while ensuring external accountability.

## 2. Business Process Flow

**Primary Actors:** Sales Manager, Stock Keeper, QA Engineer, AR Accountant, Procurement Manager.

### Process Steps:
1. **RMA Initiation:** The Sales Manager generates an RMA document linked to the original Sales Order.
2. **Inbound Receipt:** The Stock Keeper receives the goods, scans the Lot/Batch barcode, and routes them to **[Quarantine]**.
3. **QA Inspection & Root-Cause Categorization:** 
    The QA Engineer tests the item, queries the Traceability history, and assigns a Root Cause:
    *   **Category A (Vendor Defect):** Traced to faulty raw materials. *System Action:* The defect event is logged to the Historical Data Schema. The nightly Batch Engine will process this to adjust the vendor's A/B/C rating.
    *   **Category B (Internal Failure):** Traced to an internal gap in manufacturing or missed QA testing. System logs the failure for internal process review.
    *   **Category C (Customer Misuse):** Product failed due to improper handling by the end-user.
4. **Financial Resolution:** 
    *   If Category A or B: The AR Accountant issues an **AR Credit Memo** to refund the customer.
    *   If Category C: The Credit Memo is rejected. The customer is notified, and the item is either returned to them or routed for paid repair.
5. **Inventory Disposition:** Defective FG is routed to a Rework Order (if fixable) or written off via a Scrap Document.

---

## 3. Workflow Diagram

```mermaid
flowchart TD
    Start((Customer Return)) --> RMA[Sales Manager: Issue RMA]
    RMA --> Receipt[Stock Keeper: Scan Barcode and Route to Quarantine]
    
    Receipt --> QA[QA Engineer: Inspect and Query Traceability]
    QA --> Cause{Root Cause<br>Category?}
    
    %% Defect Categorization
    Cause -- A. Vendor Defect --> LogVendor[Log Defect to Historical Data Schema]
    LogVendor -.-> Batch((Nightly Batch Engine<br>Updates Vendor Risk Rating))
    LogVendor --> RefundCheck
    
    Cause -- B. Internal Failure --> LogInternal[Log Internal Process Failure] --> RefundCheck
    
    Cause -- C. Customer Misuse --> Reject[Reject Refund Request] --> Return((Return to Customer))
    
    %% Financial & Disposition
    RefundCheck[Approve Customer Refund] --> CM[AR Accountant: Issue Credit Memo]
    CM --> Disp{Disposition?}
    
    Disp -- Fixable --> Rework[Route to Rework Order]
    Disp -- Unfixable --> Scrap[Initiate Scrap Document]

    classDef default fill:#f9f9f9,stroke:#333,stroke-width:1px;
    classDef decision fill:#e1f5fe,stroke:#03a9f4,stroke-width:2px;
    class Cause,Disp decision;
    class Batch fill:#e8eaf6,stroke:#3f51b5,stroke-width:2px;
```

---

## 4. Key User Stories

*   **US-RMA-01:** As a QA Engineer, I want to query the Lot/Batch barcode of a returned item to categorise the root cause as Vendor Defect, Internal Failure, or Customer Misuse.
*   **US-RMA-02:** As the System, I want to log Vendor Defect events into the Historical Data Schema, ensuring the nightly Batch Calculation Engine accurately updates the vendor's risk label without causing synchronous system lag.
*   **US-RMA-03:** As an AR Accountant, I want the system to block the creation of an AR Credit Memo if QA categorises the defect as Customer Misuse, preventing unjustified refunds.
*   **US-RMA-04:** As the System, I want to log all Internal Failure root causes to a central QA dashboard so that management can track and improve manufacturing gaps.
