# Requirements Traceability Matrix (RTM) - Sanitised Template
**Project Reference:** NZeTA Payment & Border Integration (Portfolio Example)  
**Standard:** IEEE 29119-3 / Agile QA Delivery Framework  
**Author:** Mazher Hussain (QA Delivery Lead / Test Manager)  
**Classification:** Public Portfolio Demonstration (Synthetic Data Only)

---

## 1. Bi-Directional Traceability Matrix

| Requirement ID | Business Rule Summary | User Story | Acceptance Criteria (AC) | Test Case ID | Test Level | Automated Spec | Defect Linkage | Sign-Off Verdict |
|---|---|---|---|---|---|---|---|---|
| **BRD-PAY-001** | Multi-Currency Card Authorization via Windcave | US-PAY-101 | AC-01: Support NZD, AUD, USD, EUR, GBP | TC-PAY-001 | System Integration | `e2e/payment.spec.ts` | None | PASS |
| **BRD-PAY-002** | Asynchronous Webhook Callback Receipt | US-PAY-112 | AC-02: Webhook acknowledged within 2000ms | TC-PAY-014 | Integration / API | `api/webhook.spec.ts` | None | PASS |
| **BRD-PAY-003** | 3DS 2.0 Challenge State & Liability Shift | US-PAY-105 | AC-04: Multilingual prompt in EN and ZH | TC-3DS-008 | Security / E2E | `e2e/3ds-auth.spec.ts` | None | PASS |
| **BRD-PAY-004** | Gateway Timeout & Duplicate Payment Prevention | US-PAY-109 | AC-05: Hold automated retry on 15s timeout | TC-PAY-022 | Non-Functional / Resil | `perf/timeout.spec.ts` | DEF-4019 (Fixed) | PASS |
| **BRD-PAY-005** | Batch Financial Reconciliation Ledger | US-PAY-120 | AC-03: Zero variance between gateway and batch DB | TC-REC-003 | Data Integrity / SQL | `data/reconcile.sql` | None | PASS |

---

## 2. Traceability Governance Rules
1. **Zero Orphan Test Cases:** Every automated or manual test case must trace back to an approved Acceptance Criterion.
2. **Bi-Directional Defect Mapping:** Defects must reference the exact failing Test Case ID, User Story, and BRD clause.
3. **Audit Trail:** Code merges blocked if RTM verification coverage drops below 98%.
