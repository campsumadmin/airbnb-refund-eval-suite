# Airbnb Guest Refund Evaluation Suite

An evaluation suite for validating AI customer support agents handling Airbnb cancellation, refund, and AirCover inquiries. The suite evaluates agent responses across 20 real-world customer support scenarios against the official **Cancellation, Refund and AirCover Policy Excerpt (Effective Jan 1, 2025)**.

---

## 📋 Evaluation Suite Architecture (Five Labelled Sections in Order)

The suite is contained in a single executable Python file: [`refund_eval_suite.py`](./refund_eval_suite.py). In accordance with Stage 1 specifications, Section 1 loads the test set of 20 guest questions and booking facts with their definitive answers, while the last four sections are empty placeholders:

1. **`SECTION 1: TEST SET`**  
   Ingests and parses all 20 rows of [`airbnb_guest_refund_questions_mar2025.csv`](./airbnb_guest_refund_questions_mar2025.csv), evaluates each against booking facts, and pairs them with definitive allow/deny decisions, policy sections, and reference answers.
2. **`SECTION 2: SYSTEM PROMPT`**  
   *Empty — A later stage will fill it in.*
3. **`SECTION 3: MODEL UNDER TEST`**  
   *Empty — A later stage will fill it in.*
4. **`SECTION 4: EVALUATION METRICS`**  
   *Empty — A later stage will fill it in.*
5. **`SECTION 5: EVALUATION RUNNER`**  
   *Empty — A later stage will fill it in.*

---

## 📋 Stage 1 Verification & Check Note

```text
======================================================================================
AIRBNB REFUND EVAL SUITE - STAGE 1 VERIFICATION & CHECK NOTE
======================================================================================
Total Question-Answer Pairs Loaded: 20
Distinct Scenario Categories:        5 (all 5 present)
Scenario Types Breakdown:
  - aircover_claim          : 4 pairs
  - host_cancellation       : 4 pairs
  - late_checkout_dispute   : 4 pairs
  - non_refundable          : 4 pairs
  - partial_refund          : 4 pairs
Unclear / Hedged Answers Count:     0
======================================================================================
ID     | Scenario               | Decision | Days  | Section Used              
--------------------------------------------------------------------------------------
RQ-01  | non_refundable         | Deny     | 17    | Guest cancellation tiers  
RQ-02  | non_refundable         | Deny     | 9     | Guest cancellation tiers  
RQ-03  | non_refundable         | Deny     | 24    | Guest cancellation tiers  
RQ-04  | non_refundable         | Allow    | 26    | Service and cleaning fees 
RQ-05  | aircover_claim         | Allow    | N/A   | AirCover for guests       
RQ-06  | aircover_claim         | Allow    | N/A   | AirCover for guests       
RQ-07  | aircover_claim         | Allow    | N/A   | AirCover for guests       
RQ-08  | aircover_claim         | Deny     | N/A   | AirCover for guests       
RQ-09  | partial_refund         | Allow    | 22    | Guest cancellation tiers  
RQ-10  | partial_refund         | Allow    | 35    | Guest cancellation tiers  
RQ-11  | partial_refund         | Allow    | 16    | Guest cancellation tiers  
RQ-12  | partial_refund         | Allow    | 10    | Guest cancellation tiers  
RQ-13  | host_cancellation      | Allow    | N/A   | Host cancellations        
RQ-14  | host_cancellation      | Allow    | N/A   | Host cancellations        
RQ-15  | host_cancellation      | Allow    | N/A   | Host cancellations        
RQ-16  | host_cancellation      | Allow    | N/A   | Host cancellations        
RQ-17  | late_checkout_dispute  | Deny     | N/A   | AirCover for guests       
RQ-18  | late_checkout_dispute  | Deny     | N/A   | Guest cancellation tiers  
RQ-19  | late_checkout_dispute  | Deny     | N/A   | AirCover for guests       
RQ-20  | late_checkout_dispute  | Deny     | N/A   | AirCover for guests       
======================================================================================
CHECK NOTE STATUS: CONFIRMED - 20 PAIRS, ALL 5 TYPES, ZERO UNCLEAR ANSWERS.
======================================================================================
```

---

## 📊 Complete 20-Row Reference Table (Verified Answers & Policy Sections)

All answers are verified against the days from cancellation to check-in, `host_cancelled`, and `aircover_issue_reported`. Zero answers are vague or hedged.

| ID | Scenario Category | Booking Facts & Timeline | Decision | Policy Section Named | Unambiguous Reference Answer |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **RQ-01** | `non_refundable` | Booking: Jan 18, 2025<br>Check-in: Mar 20, 2025<br>Cancelled: Mar 3, 2025 (17 days out) | **Deny** | **Guest cancellation tiers** | Denied. Under the Non-refundable policy within 'Guest cancellation tiers', guests receive a 10% discount at booking in exchange for forfeiting refund eligibility. Cancellations for any reason, including cancelled work trips or changed travel plans, receive no refund of the nightly rate or any fee. |
| **RQ-02** | `non_refundable` | Booking: Feb 9, 2025<br>Check-in: Mar 15, 2025<br>Cancelled: Mar 6, 2025 (9 days out) | **Deny** | **Guest cancellation tiers** | Denied. The booking was confirmed under the Non-refundable rate. Under 'Guest cancellation tiers', non-refundable reservations qualify for no refund of the nightly rate or fees upon guest cancellation. Unauthorized or erroneous bot statements do not override published policy terms; no refund will be issued. |
| **RQ-03** | `non_refundable` | Booking: Jan 30, 2025<br>Check-in: Mar 28, 2025<br>Cancelled: Mar 4, 2025 (24 days out) | **Deny** | **Guest cancellation tiers** | Denied. Under 'Guest cancellation tiers' for Non-refundable bookings, cancellations for any reason—expressly including medical emergencies, personal illness, and changed plans—receive zero refund of the nightly rate or fees. |
| **RQ-04** | `non_refundable` | Booking: Feb 20, 2025<br>Check-in: Apr 2, 2025<br>Cancelled: Mar 7, 2025 (26 days out) | **Allow** *(Partial)* | **Service and cleaning fees** | Allowed in part. Under 'Service and cleaning fees', the cleaning fee is refunded whenever the guest cancels before check-in, on any tier. Because cancellation occurred on 2025-03-07 prior to the 2025-04-02 check-in (26 days in advance), the cleaning fee is refunded in full. However, the guest service fee and nightly rate are non-refundable because service fees are refunded only when a cancellation qualifies for a full refund under tier rules. |
| **RQ-05** | `aircover_claim` | Check-in: Mar 4, 2025<br>Reported: Mar 5, 2025 (~35h after check-in)<br>Issue: `no_hot_water` | **Allow** | **AirCover for guests** | Allowed. Lack of hot water is an explicitly covered issue type under 'AirCover for guests' ('No hot water or other essential amenity missing'). Because the issue was reported within 72 hours of arrival and discovery, you qualify for AirCover protection, granting rebooking assistance and a partial or full refund for the affected nights based on severity. |
| **RQ-06** | `aircover_claim` | Check-in: Mar 6, 2025<br>Reported: Mar 8, 2025 (~53h after check-in)<br>Issue: `listing_not_as_described` | **Allow** | **AirCover for guests** | Allowed. Under 'AirCover for guests', 'Listing not as described' (including severe cleanliness deficiencies and inaccessible locked rooms) is a covered issue. Because you reported it within 72 hours of arrival, you are eligible for rebooking assistance and a refund for the affected nights based on severity. |
| **RQ-07** | `aircover_claim` | Check-in: Mar 7, 2025<br>Reported: Mar 9, 2025 (~50h after check-in)<br>Issue: `could_not_check_in` | **Allow** | **AirCover for guests** | Allowed. Under 'AirCover for guests', 'Could not check in' is a covered issue when reported within 72 hours of discovery. Because the access code failed, the host was entirely unreachable, and you were forced to book alternative lodging, you qualify for a full refund of the reservation and rebooking support. |
| **RQ-08** | `aircover_claim` | Check-in: Mar 5, 2025<br>Reported: Mar 10, 2025 (5 days / 120h after check-in)<br>Issue: `pests` | **Deny** | **AirCover for guests** | Denied. Under 'AirCover for guests', claims must be reported within 72 hours of discovery. Because the pest issue was discovered on the first night (2025-03-05) and reported on 2025-03-10 (5 days / 120 hours later), the claim exceeds the strict 72-hour reporting window and is not eligible for an AirCover refund. |
| **RQ-09** | `partial_refund` | Moderate policy<br>Check-in: Mar 25, 2025<br>Cancelled: Mar 3, 2025 (22 days out) | **Allow** *(Full)* | **Guest cancellation tiers** | Allowed. Under 'Guest cancellation tiers' for Moderate listings, cancelling at least 5 days before check-in entitles the guest to a full refund of the nightly rate. Because cancellation was requested on 2025-03-03 for a 2025-03-25 check-in (22 days before check-in, exceeding the 5-day requirement), you receive a 100% full refund of the nightly rate, cleaning fee, and service fee. |
| **RQ-10** | `partial_refund` | Flexible policy<br>Check-in: Apr 10, 2025<br>Cancelled: Mar 6, 2025 (35 days out) | **Allow** *(Full)* | **Guest cancellation tiers** | Allowed. Under 'Guest cancellation tiers' for Flexible listings, cancelling at least 24 hours before check-in entitles the guest to a full refund of the nightly rate. Because you are cancelling on 2025-03-06 for a 2025-04-10 check-in (35 days before arrival), you will not lose anything and receive a 100% full refund including all fees. |
| **RQ-11** | `partial_refund` | Firm policy<br>Check-in: Mar 24, 2025<br>Cancelled: Mar 8, 2025 (16 days out) | **Allow** *(Partial)* | **Guest cancellation tiers** | Allowed in part. Under 'Guest cancellation tiers' for Firm listings, cancelling 7 to 29 days before check-in entitles the guest to a 50% refund of the nightly rate. Because cancellation occurred on 2025-03-08 for a 2025-03-24 check-in (16 days before check-in), you receive 50% of the nightly rate plus 100% of the cleaning fee. Under 'Service and cleaning fees', partial refunds do not return the guest service fee. |
| **RQ-12** | `partial_refund` | Strict policy<br>Booking: Mar 2, 2025<br>Cancelled: Mar 4, 2025 (40h)<br>Check-in: Mar 14, 2025 (10 days out) | **Allow** *(Partial)* | **Guest cancellation tiers** | Allowed in part. Under 'Guest cancellation tiers' for Strict listings, a full refund requires cancelling within 48 hours of booking AND at least 14 days before check-in. While cancelled 40 hours after booking, check-in is only 10 days away (2025-03-04 to 2025-03-14), failing the 14-day condition. Because cancellation is at least 7 days before check-in, the 50% rule applies: you receive 50% of the nightly rate and the full cleaning fee. The service fee is non-refundable. |
| **RQ-13** | `host_cancellation` | Host cancelled: True<br>Check-in: Mar 6, 2025 | **Allow** *(Full)* | **Host cancellations** | Allowed. Under 'Host cancellations', when the host cancels, the guest receives a full refund of everything paid, including the service fee. Furthermore, because the host cancelled within 30 days of check-in, AirCover will actively help you rebook a similar stay. |
| **RQ-14** | `host_cancellation` | Host cancelled: True<br>Check-in: Mar 13, 2025 | **Allow** *(Full)* | **Host cancellations** | Allowed. Under 'Host cancellations', whenever a host cancels a reservation, the guest receives a full refund of everything paid, including the nightly rate, cleaning fee, and service fee. You get 100% of your money back. |
| **RQ-15** | `host_cancellation` | Non-refundable rate<br>Host cancelled: True | **Allow** *(Full)* | **Host cancellations** | Allowed. You will not lose your money. Under 'Host cancellations', when the host cancels, the guest receives a full refund of everything paid, including the service fee. This rule applies on every tier, including Non-refundable, and overrides all tier cancellation restrictions. |
| **RQ-16** | `host_cancellation` | Host cancelled: True<br>Check-in: Mar 15, 2025 | **Allow** *(Full)* | **Host cancellations** | Allowed. Because your host cancelled the reservation, you are entitled to a 100% full refund of everything paid, including the service fee, under 'Host cancellations'. The full refund is confirmed and being processed. |
| **RQ-17** | `late_checkout_dispute` | Checkout: 11am<br>Guest left: 2pm<br>Host demanded extra night fee | **Deny** | **AirCover for guests** | Denied. Staying past the agreed checkout time (leaving at 2pm instead of 11am) is not a covered cancellation or AirCover issue. Under 'AirCover for guests', protection applies exclusively to four defined categories (check-in failure, listing not as described, essential amenities missing, or pests). Issues outside these four categories are ineligible for refunds or fee waivers under this policy. |
| **RQ-18** | `late_checkout_dispute` | In-stay departure<br>Left 1 night early voluntarily on Firm policy | **Deny** | **Guest cancellation tiers** | Denied. Under 'Guest cancellation tiers' for Firm listings, cancellations requested less than 7 days before check-in or during an ongoing stay receive no refund for unused nights. Voluntary early departure due to personal trip completion is not eligible for a refund. |
| **RQ-19** | `late_checkout_dispute` | Checkout time discrepancy<br>Host assessed late fee | **Deny** | **AirCover for guests** | Denied. Disputes regarding checkout times and host-assessed late fees do not qualify under 'AirCover for guests'. AirCover covers only four explicit issues (unable to check in, listing not as described, missing essential amenities, or pests). Any issue that is not one of these four covered types is not eligible for an AirCover refund, whatever the timing. |
| **RQ-20** | `late_checkout_dispute` | Flight delayed<br>Overstayed checkout by 1 hour | **Deny** | **AirCover for guests** | Denied. Overstaying checkout due to travel or flight delays does not fall within covered cancellation tiers or the four eligible categories of 'AirCover for guests'. Consequently, the assessed late checkout fee cannot be waived or refunded under this policy. |

---

## 🚀 Running the File

```bash
# Clone the repository
git clone https://github.com/campsumadmin/airbnb-refund-eval-suite.git
cd airbnb-refund-eval-suite

# Execute the test suite
python refund_eval_suite.py
```
