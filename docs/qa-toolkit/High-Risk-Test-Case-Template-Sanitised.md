# High-Risk Test Case Specification - Sanitised Template
**Domain:** Regulated Digital Payments & Immigration Systems  
**QA Lead:** Mazher Hussain  
**Test Objective:** Verify payment gateway timeout resilience, socket closure, and duplicate transaction prevention.

---

### Test Identifier: TC-PAY-022
- **Title:** Payment Gateway Upstream Socket Timeout (15s Jitter Window)
- **Priority:** P1 (Critical Financial Integrity)
- **Pre-Conditions:**
  - Active traveller batch application with status `AWAITING_PAYMENT`.
  - Mock Windcave gateway stub configured to inject 15,000ms socket delay.
  - Customer account funded with $180.00 NZD.

### Execution Procedure:
1. Navigate to Hosted Payment Page for Batch `NZETA-BATCH-TEST-09`.
2. Click **Submit Payment** button.
3. Assert that UI submit button is immediately disabled and displays spinner (`aria-busy="true"`).
4. Simulate rapid multi-click on disabled button. Assert that duplicate HTTP POST requests are suppressed.
5. At $T = 15.01$ seconds, intercept network response.
6. Verify API returns status `504 Gateway Timeout` with structured error code `GATEWAY_SOCKET_TIMEOUT`.
7. Assert database record transition: status set to `PENDING_RECONCILIATION`.
8. Assert zero customer credit card deduction.

### Expected Result:
- Transaction safely isolated. No duplicate charges. Clear non-technical guidance displayed to customer.
