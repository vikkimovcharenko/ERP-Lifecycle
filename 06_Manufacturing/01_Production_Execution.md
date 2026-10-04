# 06. Manufacturing Execution & Quality Assurance

## 1. Business Context
The Manufacturing module transforms raw materials into Finished Goods (FG) utilizing multi-stage production routings (e.g., Stage 1: Assembly, Stage 2: Calibration/Finishing). This scalable architecture supports End-to-End Traceability, locking the exact batch/lot numbers and receipt dates for every consumed component. 

To accommodate varying product complexities (from reworkable assemblies to monolithic components), the system allows both Production Managers and QA Engineers to initiate "Scrap" documents if irreversible defects occur at any stage. Completed batches must pass a final Quality Assurance (QA) gateway before transferring to FG inventory, while fixable defects trigger a Return to Production (Rework) order.

## 2. Business Process Flow

**Primary Actors:** Production Manager, QA Engineer, Financial Controller, System.

### Process Steps:
1. **Initiation & Allocation (Production Manager):**
    *   The Production Manager creates a **Production Work Order (WO)** based on the Bill of Materials (BOM) and multi-stage Routing template.
    *   *System Check:* Verifies "Production Available" (PA) stock and securely allocates components, locking Lot/Batch traceability.
2. **Multi-Stage Execution (WIP):**
    *   **Stage 1 (Assembly):** Components are consumed. If a monolithic part is irreparably damaged during assembly, the Production Manager can immediately initiate a **Scrap Document** for that specific item.
    *   **Stage 2 (Finishing):** The assembled unit moves to the next routing step. Status updates continuously within **[In Progress]**.
3. **QA Testing Gateway:**
    *   Upon completion of all routing stages, the batch status updates to **[In Testing]**. The QA Engineer evaluates the output.
4. **Outcome Routing:**
    *   *Scenario A (Pass):* The QA Engineer approves the batch. The system generates an FG Receipt, moving items to the FG Warehouse. Status: **[Completed]**.
    *   *Scenario B (Rework):* QA identifies fixable defects. A **Rework Order** is generated, returning the specific units to the appropriate WIP stage for correction.
    *   *Scenario C (Scrap Initialization):* QA identifies irreparable defects. The QA Engineer initiates a **Scrap Document**. 
5. **Financial Scrap Approval:**
    *   All Scrap Documents (whether initiated by Production or QA) that exceed a predefined financial threshold are routed to the Financial Controller for final write-off approval before the inventory valuation is reduced.

---

## 3. Workflow Diagram

```mermaid
flowchart TD
    Start((Create Work Order)) --> Alloc[System Allocates PA Stock<br>& Locks Lot Traceability]
    
    %% Multi-stage Routing
    Alloc --> Stage1[Stage 1: Assembly<br>Status: In Progress]
    Stage1 --> Stage2[Stage 2: Finishing<br>Status: In Progress]
    
    %% In-Process Scrap
    Stage1 -. Irreparable Damage .-> ScrapDoc[Initiate Scrap Document]
    Stage2 -. Irreparable Damage .-> ScrapDoc
    
    %% QA Process
    Stage2 --> Test[Production Complete<br>Status: In Testing]
    Test --> QA{QA Engineer<br>Inspection}
    
    QA -- Pass --> FG[Move to Finished Goods<br>Warehouse]
    FG --> End1(((Status: Completed)))
    
    QA -- Rework --> ReworkOrder[Generate Rework Order<br>Return to Stage 1 or 2]
    ReworkOrder -.-> Stage1
    
    QA -- Unfixable --> ScrapDoc
    
    %% Financial Write-off
    ScrapDoc --> FinCheck{Exceeds Financial<br>Threshold?}
    FinCheck -- Yes --> FinApprove[Financial Controller<br>Approval]
    FinCheck -- No --> WriteOff
    FinApprove --> WriteOff[System Writes-Off<br>Inventory to Scrap Account]
    WriteOff --> End2(((Scrap Processed)))

    classDef default fill:#f9f9f9,stroke:#333,stroke-width:1px;
    classDef decision fill:#e1f5fe,stroke:#03a9f4,stroke-width:2px;
    classDef sysAction fill:#f3e5f5,stroke:#9c27b0,stroke-width:2px;
    classDef highlight fill:#fff9c4,stroke:#fbc02d,stroke-width:2px;
    
    class QA,FinCheck decision;
    class Alloc,WriteOff sysAction;
    class Stage1,Stage2 highlight;

```

---

## 4. Key User Stories

  *   **US-MFG-01:** As a Production Manager, I want the Work Order to follow a multi-stage routing path (e.g., Assembly then Finishing), so that I can track work-in-progress accurately across different factory departments.
  *   **US-MFG-02:** As a Production Manager, I want the ability to initiate a Scrap Document during the assembly stage for monolithic parts that break, so that inventory is immediately corrected without waiting for final QA.
  *   **US-QA-01:** As a QA Engineer, I want the ability to partially reject a production batch and trigger a Rework Order targeting a specific prior manufacturing stage, so that fixable products are efficiently corrected.
  *   **US-QA-02:** As a QA Engineer, I want to initiate a Scrap Document for completed units that fail final inspection and cannot be reworked, ensuring defective items never reach Finished Goods.
  *   **US-FIN-02:** As a Financial Controller, I want to approve any Scrap Documents exceeding our financial threshold, so that high-value inventory write-offs are financially audited before posting to the general ledger.





