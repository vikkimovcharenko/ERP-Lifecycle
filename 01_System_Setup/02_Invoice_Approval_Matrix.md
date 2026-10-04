# 02. AP Invoice Approval Matrix

## 1. Business Context
The Accounts Payable (AP) Approval Matrix defines the delegation of authority for authorizing vendor payments. It is triggered after a successful 3-Way Match (PO = Goods Receipt = Invoice) or after a tolerated Purchase Price Variance (PPV) is registered. The matrix ensures financial control, separating invoice registration from final payment execution.

## 2. Approval Matrix Rules

| Step | Role | Condition / Threshold | Action |
| :--- | :--- | :--- | :--- |
| **1. Registration** | AP Accountant | All Invoices | Validates physical document against system match (Status: `Initiated/New`) |
| **2. Exception Routing** | Procurement Manager | Price Variance exceeds tolerance limits | Resolves discrepancies; requests Credit Memo (Status: `Price Hold`) |
| **3. Tier 1 Approval** | AP Manager | Invoices ≤ €10,000 (Within match/tolerance) | Financial validation and budget confirmation |
| **4. Tier 2 Approval** | Financial Controller | Invoices > €10,000 to €50,000 | Secondary review for high-value operational spend |
| **5. Tier 3 Approval** | CFO | Invoices > €50,000 | Executive sign-off |
| **6. Execution** | Payment Manager | Approved AP Batches | Authorizes API transmission to the bank for payment release |

## 3. Key User Stories
*   **US-APM-01:** As the System, I want to automatically route matched invoices under €10,000 directly to the AP Manager, so that low-value payments are processed efficiently.
*   **US-APM-02:** As a Payment Manager, I want to see only fully approved invoices in my payment queue, ensuring compliance with the company's Delegation of Authority (DOA).

