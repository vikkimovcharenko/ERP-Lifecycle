# 02. Customer Returns (RMA) & Traceability Resolution

## 1. Business Context
When a customer returns Finished Goods (FG), the Return Merchandise Authorization (RMA) process manages the inbound logistics, financial resolution, and root-cause analysis. Leveraging End-to-End Lot/Batch Traceability, the system categorizes the defect origin into three streams: **Vendor Failure** (raw materials), **Internal QA/Production Failure**, or **Customer Misuse**. This categorization dictates whether the customer receives a refund (Credit Memo) and whether internal or external corrective actions are triggered.

## 2. Business Process Flow

**Primary Actors:** Sales Manager, Stock Keeper, QA Engineer, AR Accountant, Procurement Manager.

### Process Steps:
1. **RMA Initiation:** The Sales Manager generates an RMA document linked to the original Sales Order.
2. **Inbound Receipt:** The Stock Keeper receives the goods, scans the Lot/Batch barcode, and routes them to **[Quarantine]**.
3. **QA Inspection & Root-Cause Categorization:** 
    The QA Engineer tests the item, queries the Traceability history, and assigns a Root Cause:
    *   **Category A (Vendor Defect):** Traced to faulty raw materials. System penalizes the Vendor Rating and alerts Procurement.
    *   **Category B (Internal Failure):** Traced to a gap in internal manufacturing or missed QA testing. System logs the failure for internal process review.
    *   **Category C (Customer Misuse):** Product failed due to improper handling by the end-user.
4. **Financial Resolution:** 
    *   If Category A or B: The AR Accountant issues an **AR Credit Memo** to refund the customer.
    *   If Category C: The Credit Memo is rejected. The customer is notified, and the item is either returned to them or routed for paid repair.
5. **Inventory Disposition:** Defective FG is routed to a Rework Order or written off via a Scrap Document.

---

## 3. Workflow Diagram

```mermaid
flowchart TD
    Start((Customer Return)) --> RMA[Sales Manager: Issue RMA]
    RMA --> Receipt[Stock Keeper: Scan Lot/Batch<br>& Move to Quarantine]
    
    Receipt --> QA[QA Engineer: Inspect &<br>Query Traceability]
    QA --> RootCause{Root Cause<br>Category?}
    
    %% Category A: Vendor
    RootCause -- A. Vendor Defect --> Alert[Penalize Vendor Rating<br>& Alert Procurement]
    Alert --> RefundCheck
    
    %% Category B: Internal
    RootCause -- B. Internal Failure --> IntLog[Log Internal QA/MFG Failure]
    IntLog --> RefundCheck
    
    %% Category C: Customer Misuse
    RootCause -- C. Customer Misuse --> RejectRefund[Reject Refund Request]
    RejectRefund --> End1(((Return to Customer /<br>Paid Repair)))
    
    %% Disposition & Finance for Valid Returns
    RefundCheck[Approve Customer Refund] --> AR[AR Accountant:<br>Issue AR Credit Memo]
    AR --> QADecision{Fixable?}
    
    QADecision -- Yes --> Rework[Route to Rework]
    QADecision -- No --> Scrap[Initiate Scrap Document]

    classDef default fill:#f9f9f9,stroke:#333,stroke-width:1px;
    classDef decision fill:#e1f5fe,stroke:#03a9f4,stroke-width:2px;
    classDef sysAction fill:#f3e5f5,stroke:#9c27b0,stroke-width:2px;
    classDef highlight fill:#fff9c4,stroke:#fbc02d,stroke-width:2px;
    
    class RootCause,QADecision decision;
    class Alert,IntLog sysAction;
    class QA highlight;
```

___

## 4. Key User Stories

*   **US-RMA-01:** As a QA Engineer, I want to query the Lot/Batch barcode of a returned item to categorize the root cause as Vendor Defect, Internal Failure, or Customer Misuse.
*   **US-RMA-02:** As the System, I want to automatically update the Vendor Rating and alert Procurement if an RMA is categorized as a Vendor Defect.
*   **US-RMA-03:** As an AR Accountant, I want the system to block the creation of an AR Credit Memo if QA categorizes the defect as Customer Misuse, preventing unjustified refunds.
*   **US-RMA-04:** As the System, I want to log all Internal Failure root causes to a central QA dashboard so that management can track and improve manufacturing gaps.

  
