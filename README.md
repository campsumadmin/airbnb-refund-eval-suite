# Airbnb Guest Refund Evaluation Suite

An end-to-end evaluation suite for validating AI customer support agents handling Airbnb cancellation, refund, and AirCover inquiries. The suite evaluates agent responses across 20 real-world customer support scenarios against the official **Cancellation, Refund and AirCover Policy Excerpt (Effective Jan 1, 2025)**.

---

## 📋 Evaluation Suite Architecture (5 Labelled Sections)

The core evaluation harness is implemented in a single self-contained file: [`refund_eval_suite.py`](./refund_eval_suite.py), structured into five distinct, labeled sections:

1. **`SECTION 1: TEST SET & DATA LOADING`**
   - Ingests and parses all 20 rows from [`airbnb_guest_refund_questions_mar2025.csv`](./airbnb_guest_refund_questions_mar2025.csv).
   - Maps raw CSV fields into strongly-typed `RefundTestCase` data classes with strict type casting for dates, floats, and booleans.

2. **`SECTION 2: GROUND TRUTH REFERENCE DATA & POLICY MAPPING`**
   - Defines authoritative ground truth for all 20 test cases.
   - Maps every query to:
     - **Outcome Tag**: `allow_full`, `allow_partial`, or `deny`
     - **Policy Section Used**: One of `Guest cancellation tiers`, `Service and cleaning fees`, `Host cancellations`, or `AirCover for guests`.
     - **Reference Answer**: Legally grounded, policy-accurate response citing specific terms and calculations.
     - **Key Evidence Points**: Fact check items required in model explanations.

3. **`SECTION 3: SYSTEM PROMPT & AGENT UNDER TEST`**
   - `REFUND_SYSTEM_PROMPT`: Directs candidate models to evaluate claims against policy tiers and output structured JSON (`outcome_tag`, `section_used`, `answer`).
   - `PolicyEngineAgent`: Deterministic reference baseline implementing policy rules, used as the benchmark standard.

4. **`SECTION 4: EVALUATION METRICS & SCORERS`**
   - `score_outcome_tag`: Validates exact classification match (`allow_full`, `allow_partial`, `deny`).
   - `score_policy_section`: Validates correct policy section citation.
   - `compute_metrics`: Computes overall accuracy, section citation accuracy, slice pass rates, and a multi-class confusion matrix.

5. **`SECTION 5: EVALUATION RUNNER & BENCHMARK REPORTING`**
   - Orchestrates the test loop across all 20 test cases.
   - Displays a live ASCII verification table.
   - Computes category-level accuracy breakdowns.
   - Serializes execution traces and benchmark metrics to [`refund_eval_results.json`](./refund_eval_results.json).

---

## 📊 Complete 20-Row Reference Table & Policy Citations

| ID | Scenario Category | Outcome Tag | Policy Section Used | Reference Answer |
| :--- | :--- | :--- | :--- | :--- |
| **RQ-01** | `non_refundable` | `deny` | **Guest cancellation tiers** | Under the Non-refundable policy, guests who chose the 10% discount at booking receive no refund of the nightly rate or fees when cancelling for any reason, including cancelled work trips or changed travel plans. Your refund request is denied. |
| **RQ-02** | `non_refundable` | `deny` | **Guest cancellation tiers** | Your reservation was booked under a Non-refundable rate. According to the policy, non-refundable bookings receive no refund of the nightly rate or fees upon guest cancellation, regardless of prior incorrect statements made by automated bots. The refund cannot be processed. |
| **RQ-03** | `non_refundable` | `deny` | **Guest cancellation tiers** | While we sympathize with family medical emergencies, the Non-refundable cancellation policy explicitly states that cancellations for any reason—including personal illness and changed plans—are not eligible for any refund of the nightly rate or fees. |
| **RQ-04** | `non_refundable` | `allow_partial` | **Service and cleaning fees** | Under the policy, the cleaning fee is refunded whenever the guest cancels before check-in on any tier, including Non-refundable. However, the guest service fee and nightly rate are non-refundable because the service fee is only refunded when a cancellation qualifies for a full refund under tier rules. Therefore, you receive a partial refund consisting solely of the cleaning fee. |
| **RQ-05** | `aircover_claim` | `allow_partial` | **AirCover for guests** | Lack of hot water is an explicitly covered issue type under AirCover ('No hot water or other essential amenity missing'). Because it was reported within 72 hours of discovery, you are eligible for AirCover protection, which provides rebooking assistance or a full/partial refund for the affected nights based on severity. |
| **RQ-06** | `aircover_claim` | `allow_partial` | **AirCover for guests** | Under AirCover for guests, 'Listing not as described' (including severe cleanliness issues and locked rooms) is a covered issue. Because you reported it within 72 hours of arrival, you are eligible for rebooking assistance or a partial/full refund for affected nights depending on severity. |
| **RQ-07** | `aircover_claim` | `allow_full` | **AirCover for guests** | Under AirCover for guests, 'Could not check in' is a covered issue when reported within 72 hours. Because you were entirely unable to access the listing and had to book alternative lodging due to no host response, you are entitled to a full refund for the affected stay as well as rebooking support. |
| **RQ-08** | `aircover_claim` | `deny` | **AirCover for guests** | Although pests are a covered category under AirCover, the policy explicitly dictates that the issue must be reported within 72 hours of discovery. Because you discovered the roaches on your first night and reported them more than 72 hours later (5 days into the stay), this claim is ineligible for an AirCover refund. |
| **RQ-09** | `partial_refund` | `allow_full` | **Guest cancellation tiers** | Under the Moderate cancellation policy, cancelling at least 5 days before check-in entitles you to a full refund of the nightly rate. Because you are cancelling 22 days before check-in, you receive a full refund of the nightly rate, the cleaning fee, and the guest service fee. |
| **RQ-10** | `partial_refund` | `allow_full` | **Guest cancellation tiers** | Under the Flexible cancellation policy, cancelling at least 24 hours prior to check-in qualifies for a full refund of the nightly rate. Because check-in is over a month away, you receive a 100% full refund, including the guest service fee and cleaning fee. |
| **RQ-11** | `partial_refund` | `allow_partial` | **Guest cancellation tiers** | Under the Firm cancellation policy, cancelling 7 to 29 days before check-in qualifies for a 50% refund of the nightly rate plus a full refund of the cleaning fee. The service fee is not refunded for partial cancellations. Because you are cancelling 16 days prior to arrival, you receive a partial refund. |
| **RQ-12** | `partial_refund` | `allow_partial` | **Guest cancellation tiers** | Under the Strict cancellation policy, a full refund requires cancelling within 48 hours of booking AND at least 14 days before check-in. Although you booked 40 hours ago, check-in is only 10 days out (< 14 days). Therefore, the 50% refund rule applies (cancelled at least 7 days before check-in). You receive 50% of the nightly rate plus the cleaning fee; the service fee is non-refundable. |
| **RQ-13** | `host_cancellation` | `allow_full` | **Host cancellations** | When the host cancels a reservation, the guest receives a full refund of everything paid, including the service fee. Additionally, because the cancellation occurred within 30 days of check-in, AirCover assists you with rebooking a similar stay. |
| **RQ-14** | `host_cancellation` | `allow_full` | **Host cancellations** | Yes, you get everything back. Under the Host cancellations policy, when the host cancels, the guest receives a full refund of all amounts paid, including nightly rates, cleaning fees, and service fees. |
| **RQ-15** | `host_cancellation` | `allow_full` | **Host cancellations** | No, you will not lose your money. The Host cancellations policy explicitly states that when the host cancels, the guest receives a full refund of everything paid, including the service fee. This rule applies on every tier, including Non-refundable, and overrides tier restrictions. |
| **RQ-16** | `host_cancellation` | `allow_full` | **Host cancellations** | Because your host cancelled the reservation, you are entitled to a full refund of all amounts paid, including the service fee. We will verify that the full refund is processed to your original payment method. |
| **RQ-17** | `late_checkout_dispute` | `deny` | **AirCover for guests** | Under the policy excerpt, staying past checkout time is not a covered cancellation or AirCover issue. AirCover protects only against four specific issues (check-in failure, listing not as described, missing essential amenities, and pests). Late checkout disputes and fees are not eligible for a refund or waiver under this policy. |
| **RQ-18** | `late_checkout_dispute` | `deny` | **Guest cancellation tiers** | Under the Firm cancellation policy, cancellations made less than 7 days before check-in or during an ongoing stay receive no refund for unused nights. Personal decisions to conclude a trip early do not qualify for a refund of the remaining nights. |
| **RQ-19** | `late_checkout_dispute` | `deny` | **AirCover for guests** | Disputes regarding host late checkout fees are not covered under the four eligible AirCover categories (could not check in, listing not as described, missing essential amenities, or pests). Issues outside these four categories are not eligible for an AirCover refund. |
| **RQ-20** | `late_checkout_dispute` | `deny` | **AirCover for guests** | While travel and flight delays are frustrating, unauthorized late checkout fees are not eligible for refunds or waivers under the policy excerpt. AirCover only covers the four designated issues, and flight delays do not qualify for fee refunds under this policy. |

---

## 📈 Benchmark Execution & Verification

Running the test runner produces:

```text
================================================================================
AIRBNB GUEST REFUND EVALUATION SUITE (MARCH 2025 DATASET)
Loaded 20 test cases from test set.
================================================================================

ID     | Category               | Expected      | Predicted     | Outcome | Section | Status
----------------------------------------------------------------------------------------
RQ-01  | non_refundable         | deny          | deny          | MATCH   | MATCH   | PASS  
RQ-02  | non_refundable         | deny          | deny          | MATCH   | MATCH   | PASS  
RQ-03  | non_refundable         | deny          | deny          | MATCH   | MATCH   | PASS  
RQ-04  | non_refundable         | allow_partial | allow_partial | MATCH   | MATCH   | PASS  
RQ-05  | aircover_claim         | allow_partial | allow_partial | MATCH   | MATCH   | PASS  
RQ-06  | aircover_claim         | allow_partial | allow_partial | MATCH   | MATCH   | PASS  
RQ-07  | aircover_claim         | allow_full    | allow_full    | MATCH   | MATCH   | PASS  
RQ-08  | aircover_claim         | deny          | deny          | MATCH   | MATCH   | PASS  
RQ-09  | partial_refund         | allow_full    | allow_full    | MATCH   | MATCH   | PASS  
RQ-10  | partial_refund         | allow_full    | allow_full    | MATCH   | MATCH   | PASS  
RQ-11  | partial_refund         | allow_partial | allow_partial | MATCH   | MATCH   | PASS  
RQ-12  | partial_refund         | allow_partial | allow_partial | MATCH   | MATCH   | PASS  
RQ-13  | host_cancellation      | allow_full    | allow_full    | MATCH   | MATCH   | PASS  
RQ-14  | host_cancellation      | allow_full    | allow_full    | MATCH   | MATCH   | PASS  
RQ-15  | host_cancellation      | allow_full    | allow_full    | MATCH   | MATCH   | PASS  
RQ-16  | host_cancellation      | allow_full    | allow_full    | MATCH   | MATCH   | PASS  
RQ-17  | late_checkout_dispute  | deny          | deny          | MATCH   | MATCH   | PASS  
RQ-18  | late_checkout_dispute  | deny          | deny          | MATCH   | MATCH   | PASS  
RQ-19  | late_checkout_dispute  | deny          | deny          | MATCH   | MATCH   | PASS  
RQ-20  | late_checkout_dispute  | deny          | deny          | MATCH   | MATCH   | PASS  

================================================================================
SUMMARY METRICS
================================================================================
Total Test Cases:           20
Outcome Tag Accuracy:       100.0%
Policy Section Accuracy:    100.0%
Full Pass Rate (Both):      100.0%

--------------------------------------------------------------------------------
PER-CATEGORY BREAKDOWN
--------------------------------------------------------------------------------
Category                  | Cases  | Outcome Acc  | Section Acc  | Pass Rate 
--------------------------------------------------------------------------------
aircover_claim            | 4      |      100.0% |      100.0% |    100.0%
host_cancellation         | 4      |      100.0% |      100.0% |    100.0%
late_checkout_dispute     | 4      |      100.0% |      100.0% |    100.0%
non_refundable            | 4      |      100.0% |      100.0% |    100.0%
partial_refund            | 4      |      100.0% |      100.0% |    100.0%
```

---

## 🚀 Running Locally

```bash
# Clone the repository
git clone https://github.com/campsumadmin/airbnb-refund-eval-suite.git
cd airbnb-refund-eval-suite

# Execute the evaluation suite
python refund_eval_suite.py
```
