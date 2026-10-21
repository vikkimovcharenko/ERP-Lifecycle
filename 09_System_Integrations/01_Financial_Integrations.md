# 09. System Integrations & Interfaces

## 1. Financial Integrations: Outbound AP Payments (REST API & Webhooks)

### 1.1 Process Trigger & Pre-Flight Checks
*   **Automation Rule:** A scheduled CRON job executes hourly between 09:00 and 16:00.
*   **Selection Criteria:** Sweeps the AP Ledger for `Invoice_Status = Approved` AND `Payment_Due_Date <= Current_Date`.
*   **Pre-Flight Validation:**
    *   Vendor has an active IBAN and valid SWIFT/BIC code.
    *   Invoice Currency matches Vendor Bank Account Currency.
    *   *Exception Handling:* If checks fail, status updates to `Exception: Missing Master Data` and an alert is sent.

### 1.2 API Architecture & Idempotency Rules
*   **Integration Method:** RESTful API with Asynchronous Webhooks.
*   **Synchronous Handshake:** The Payment Engine sends a `POST /payments` request to the Bank. The bank responds with `HTTP 202 Accepted`, confirming schema validation (this does not mean funds have settled).
*   **Inbound Endpoint (Webhook):** The ERP hosts a public-facing, authenticated endpoint (`https://api.[company].com/webhooks/payment-update`) to receive status updates.
*   **Webhook Acknowledgement:** The Payment Engine MUST immediately return an `HTTP 200 OK` upon receiving the webhook to prevent bank API timeouts and retry loops.
*   **Idempotency Key Validation:** Because network glitches may cause duplicate webhooks, the Payment Engine must validate the `Transaction_ID` + `Status` combination against the database before updating the ERP. If the combination already exists, the webhook is ignored.

### 1.3 Invoice Status Lifecycle (State Machine)
1.  **Approved:** Ready for selection.
2.  **In Transit:** Batch approved by AP Manager, `HTTP 202` received from Bank.
3.  **Paid:** Webhook received confirming settlement. Idempotency verified. AP sub-ledger cleared.
4.  **Payment Failed:** Webhook received indicating failure (e.g., NSF, invalid account). Alert routed to AP Manager.

---

### 1.4 API Sequence Diagram

[UML Sequence Diagram: AP Payments](.09_System_Integrations/Diagrams/UML_Diagram_AP_Payments.png)
