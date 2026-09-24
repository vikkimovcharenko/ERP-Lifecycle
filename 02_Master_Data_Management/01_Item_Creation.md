# 02. Item Master Data Creation (MDM)

## 1. Business Context
The creation of new items (nomenclature) is decoupled from the transactional Procurement process to maintain data integrity and enforce Separation of Duties (SoD). This process ensures all new items are created with mandatory parameters (Inventory/WIP limits, Quality Control) before they can be used in Requisitions.

## 2. Business Process Flow: Item Creation Request

**Primary Actors:** Requestor (e.g., Production Manager), Item Admin (MDM Role)

### Process Steps:
1. **Initiation:** The Requestor navigates to a dedicated system block: *System Requests*.
2. **Form Entry:** The Requestor selects *Add Item* and populates the following mandatory fields:
    *   Short Description
    *   Full Description
    *   Units of Measurement (UOM)
    *   Cost Center
    *   Inventory Limits (Min/Max stock levels)
    *   WIP Limits (Work-in-Progress constraints)
    *   Quality Control parameters
3. **Submission:** The Requestor clicks *Save Request* and then *Send Request*. 
    *   *System Action:* The system updates the request status to **[Sent]**.
4. **Admin Review:** The Item Admin receives the request in their queue and reviews the provided data (performing standard duplicate checks).
5. **Execution & Closure:** The Item Admin adds the new item to the master catalog based on the request details.
    *   *System Action:* Upon successful creation of the item, the system automatically changes the original Request status to **[Done]**.

---

## 3. Workflow Diagram

```mermaid
flowchart TD
    Start((Need New Item)) --> A[Open 'System Requests' Module]
    A --> B[Click 'Add Item']
    
    B --> C[Fill Mandatory Fields:<br>Descriptions, UOM, CC,<br>Inv/WIP Limits, QC]
    C --> D[Save & Send Request]
    
    D --> E[[Status: Sent]]
    
    E --> F[Item Admin Receives Request]
    F --> G{Duplicate Check}
    
    G -- Match Found --> Reject[Reject Request] --> End1(((Closed)))
    
    G -- No Match --> H[Admin Adds Item to Master Catalog]
    H --> I[[Status: Done]]
    I --> End2(((Item Ready for PR)))

    classDef default fill:#f9f9f9,stroke:#333,stroke-width:1px;
    classDef status fill:#fff3e0,stroke:#ff9800,stroke-width:2px;
    classDef statusDone fill:#e8f5e9,stroke:#4caf50,stroke-width:2px;
    classDef decision fill:#e1f5fe,stroke:#03a9f4,stroke-width:2px;
    
    class E status;
    class I statusDone;
    class G decision;
