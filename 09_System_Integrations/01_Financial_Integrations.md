# 09. System Integrations & Interfaces

## 1. Financial Integrations: Outbound AP Payments

### 1.1 Process Trigger & Scheduling
*   **Automation Rule:** A scheduled CRON job executes hourly between 09:00 and 16:00 (system local time).
*   **Selection Criteria:** The system sweeps the AP Ledger for all invoices where `Invoice_Status = Approved` AND `Payment_Due_Date <= Current_Date`.
*   **Pre-Flight Validation Rule:** Before selection into the batch, the system verifies:
    *   Vendor has an active `IBAN`.
    *   Vendor has a valid `SWIFT/BIC` code.
    *   Invoice Currency matches Vendor Bank Account Currency.
    *   *Exception Handling:* If any check fails, the invoice status updates to `Exception: Missing Master Data`, it is excluded from the batch, and an alert is routed to the AP Manager.

### 1.2 Format & Payload Specification
*   **Integration Method:** Secure Host-to-Host (H2H) SFTP connection.
*   **Standard:** ISO 20022 Global Banking Format.
*   **Outbound File (`pain.001`):** Payment Initiation message. The system aggregates all selected invoices into a single encrypted XML batch file.
*   **Inbound Feedback (`pain.002`):** Payment Status Report received from the bank to confirm acceptance or rejection of the batch.

### 1.3 Invoice Status Lifecycle (State Machine)
To prevent duplicate payments across hourly batches, the invoice status follows strict transitions:
1.  **Approved:** Invoice has passed the 3-Way Match and Approval Matrix. Ready for selection.
2.  **In Transit (Locked):** The hourly batch sweeps the invoice into the `pain.001` file. The status updates immediately to prevent the next hourly job from picking it up.
3.  **Paid (Closed):** The system processes the bank's `pain.002` acknowledgement file confirming successful disbursement. The AP sub-ledger is cleared.
4.  **Failed (Exception):** The bank rejects the payment (e.g., invalid vendor IBAN or insufficient funds). The status updates to `Payment Failed`, and an alert is routed to the AP Manager.

### 1.4 Data Mapping Document (pain.001 Source-to-Target)

| XML Tag (Target) | Bank Field Description | ERP Source Data & Transformation Rule |
| :--- | :--- | :--- |
| `<ReqdExctnDt>` | Requested Execution Date | `Payments.Payment_Date`. *Rule: If NULL, default to `Current_System_Date`.* |
| `<Dbtr><Nm>` | Debtor (Payer) Name | `System_Setup.Company_Legal_Name` |
| `<Cdtr><Nm>` | Creditor (Payee) Name | `Vendor_Master.Legal_Supplier_Full_Name` |
| `<CdtrAcct><Id><IBAN>` | Creditor IBAN | `AP_Invoices.Planned_Payments.Vendor_Account_IBAN` |
| `<InstdAmt Ccy="EUR">` | Instructed Amount & Currency | `AP_Invoices.Planned_Payments.Amount` & `Currency_Code` |
| `<RmtInf><Ustrd>` | Unstructured Remittance Info | `AP_Invoices.Planned_Payments.Payment_Reference` *(Format: [Vendor_Invoice_No] / [ERP_Invoice_ID])* |
