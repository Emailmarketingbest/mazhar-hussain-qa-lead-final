# Critical Defect Investigation Report - Sanitised Template
**Defect Reference:** DEF-4019  
**Severity:** P1 Blocker  
**Component:** Async Payment Webhook Handler  
**Investigator:** Mazher Hussain (QA Delivery Lead)  
**Status:** Resolved & Verified in Release Candidate 3.0

---

## 1. Executive Summary
During multi-threaded stress testing of Build 2.1, 3 out of 100 concurrent payments experienced a condition where cardholders were billed by Windcave, but the immigration application remained in `Unpaid` status.

## 2. Root Cause Analysis (5-Whys)
1. **Why was application marked unpaid?** The backend API never processed the payment confirmation.
2. **Why was confirmation not processed?** The incoming webhook callback returned HTTP 500.
3. **Why did webhook return 500?** A transient database connection pool exhaustion caused a SQL timeout.
4. **Why was webhook not retried?** The webhook retry queue lacked backoff jitter and dropped dropped packets after 1 attempt.
5. **Root Cause:** Inadequate dead-letter queue (DLQ) retry configuration and missing fallback status inquiry cron.

## 3. Remediation & Verification
- **Code Fix:** Implemented exponential backoff retry policy (3 retries at 10s, 30s, 90s) and fallback polling cron job every 10 minutes.
- **Regression Suite:** Injected 250 simulated network dropped packets across 500 concurrent payments.
- **Result:** 100% of dropped callbacks recovered automatically within 2 minutes. Zero orphaned payments.
