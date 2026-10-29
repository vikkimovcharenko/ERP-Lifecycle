
# 301. Purchase Requisition (PR) Management

## 1. Prerequisites (System Setup & Data)
Before a Purchase Requisition can be successfully initiated and routed, the following system configurations and master data migrations must be completed.

### 1.1. Security & Role Setup
*   **Production Manager:** Granted permissions to create Purchase Requisitions (PR) and initiate "Item Creation" requests.
*   **Item Admin / MDM:** Granted permissions to create, update, and manage Item Master Data.
*   **Procurement Manager:** Granted permissions to create and manage Vendors, Contracts, and Purchase Orders (PO).

### 1.2. Master Data & Financial Setup
*   **Organizational Structure:** Employee list uploaded with reporting hierarchies (essential for approval routing).
*   **Financials:** Financial info configured, including GL Accounts and Cost Centers.
*   **Workflow:** Procurement Approval Matrix configured in the system.
*   **Catalogs:** Initial upload of Item Master Data, Vendors, and Contracts.
*   **Cutover Data (Migration):** Upload of historical/open data, including open PRs, open POs, on-hand inventory, and Work-in-Progress (WIP) inventory.

---
## 2. Business Process Flow: Create Purchase Requisition

**Primary Actors:** Production Manager (Requestor), System (MRP)
**Trigger:** Need for materials (Part X) for the manufacturing process.

**Trigger Gateway (Manual vs. Automated based on Risk Label):**
*   **Automated (MRP/Min-Max - Category A Vendors Only):** For standard catalogue components linked to a Category A (Green) vendor, the MRP engine continuously monitors stock levels. When inventory drops below the predefined Minimum Stock threshold, the system automatically generates a PR.
*   **Manual (Category B Vendors):** If the primary vendor is Category B (Yellow), the MRP halts automation and alerts the Production Manager to manually initiate the PR or route the demand to a Secondary Vendor.
*   **Blocked (Category C Vendors):** No PRs can be generated for Category C (Red) vendors. 

### Process Steps:
1. **Initiation:** The Requestor navigates to the *Requisitions* module and selects *Create Requisition*.
2. **Vendor & Contract Selection (Conditional):**
    *   *Scenario A (Known Source):* The Requestor selects an existing Vendor and an active Contract.
    *   *Scenario B (Unknown Source):* If the vendor or contract does not exist, the Requestor leaves these fields blank. (Note: This will later trigger a separate Sourcing/RFQ process for the Procurement department).
3. **Item Selection:**
    *   If *Scenario A* was selected, choose the item directly from the specific Vendor’s catalog.
    *   If *Scenario B* was selected, choose the item from the general Item Master catalog.
    *   *Exception Handling:* If the required item does not exist in the system at all, the Requestor pauses the PR creation and initiates a separate **Item Creation Request** workflow.
4. **Submission & Routing:** The Requestor clicks *Send for Approval*. The system evaluates the PR value and Vendor Risk category, routing it based on the Procurement Approval Matrix.
5. **Exception Handling (Orphaned Demand):** If an active PR's linked vendor is downgraded to Category C (Red) before the PO is dispatched, the system automatically revokes the vendor, changes the PR status to **[Sourcing Required]**, and alerts the Procurement Manager to find a new supplier.

### Process Diagram



---

## 3. Key User Stories

*   **US-PR-01:** As a Production Manager, I want to create a purchase requisition for an item without specifying a vendor, so that the Procurement team can source and negotiate the best contract.
*   **US-PR-02:** As a Production Manager, I want to easily select an item from an existing vendor's active contract, so that the procurement process is expedited.
*   **US-PR-03:** As a Production Manager, I need the ability to trigger an "Item Creation Request" directly from the PR screen if a part is missing, so that I don't have to navigate to a different module to request master data updates.
*   **US-PR-04:** As the System, I want to automatically generate a Purchase Requisition when an active item's stock falls below the minimum threshold, so that manufacturing never halts due to predictable material shortages.



