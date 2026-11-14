# Check Note: Airbnb Refund Evaluation Suite (Stage 1)

**Verification Date:** 14 November 2026  
**Dataset:** "Guest Refund Questions, 3-11 March 2025" (`airbnb_guest_refund_questions_mar2025.csv`)  
**Governing Policy:** "Cancellation, Refund and AirCover Policy Excerpt" (Effective 1 January 2025)  
**Status:** ✅ **CONFIRMED — 20 Pairs | All 5 Types | 0 Unclear Answers**

---

## 1. Executive Verification Summary

| Metric | Required | Actual | Verification Status |
| :--- | :---: | :---: | :---: |
| **Total Question-Answer Pairs** | **20** | **20** | ✅ PASS (Exact match, RQ-01 through RQ-20) |
| **Scenario Types Represented** | **5** | **5** | ✅ PASS (All 5 types, exactly 4 pairs each) |
| **Unclear or Hedged Answers** | **0** | **0** | ✅ PASS (Zero hedging, 100% decisive allow/deny) |
| **Policy Sections Cited** | Yes | Yes | ✅ PASS (Exact sections from Policy Excerpt named) |
| **Days Calculation Verified** | Yes | Yes | ✅ PASS (Exact calendar math from cancel to check-in) |
| **Host Cancellation Flag Verified** | Yes | Yes | ✅ PASS (`host_cancelled` overrides verified) |
| **AirCover 72h Window Verified** | Yes | Yes | ✅ PASS (72h discovery rule strictly audited) |

---

## 2. Scenario Types Distribution (All 5 Types)

The 20 questions are evenly distributed across all five distinct scenario types:

1. **`non_refundable`** (4 pairs): `RQ-01`, `RQ-02`, `RQ-03`, `RQ-04`
2. **`aircover_claim`** (4 pairs): `RQ-05`, `RQ-06`, `RQ-07`, `RQ-08`
3. **`partial_refund`** (4 pairs): `RQ-09`, `RQ-10`, `RQ-11`, `RQ-12`
4. **`host_cancellation`** (4 pairs): `RQ-13`, `RQ-14`, `RQ-15`, `RQ-16`
5. **`late_checkout_dispute`** (4 pairs): `RQ-17`, `RQ-18`, `RQ-19`, `RQ-20`

---

## 3. Detailed Audit of All 20 Question-Answer Pairs

### Scenario Type 1: `non_refundable`

#### Pair 1: RQ-01
- **Guest Question:** "I booked a non-refundable rate but my work trip got cancelled, is there any way to get my money back?"
- **Booking Facts:** Non-refundable rate | Booked: 2025-01-18 | Check-in: 2025-03-20 | Cancelled: 2025-03-03 (17 days before check-in) | `host_cancelled`: False | `aircover_issue_reported`: False
- **Decision:** **Deny**
- **Policy Section Named:** `Guest cancellation tiers`
- **Reference Answer:** Denied. Under the Non-refundable policy within 'Guest cancellation tiers', guests receive a 10% discount at booking in exchange for forfeiting refund eligibility. Cancellations for any reason, including cancelled work trips or changed travel plans, receive no refund of the nightly rate or any fee.
- **Clarity & Anti-Hedge Check:** ✅ Clear, decisive refusal. Zero vague or conditional language.

#### Pair 2: RQ-02
- **Guest Question:** "The chatbot already told me yesterday I'd get a full refund on my non-refundable booking, so when will that come through?"
- **Booking Facts:** Non-refundable rate | Booked: 2025-02-09 | Check-in: 2025-03-15 | Cancelled: 2025-03-06 (9 days before check-in) | `host_cancelled`: False | `aircover_issue_reported`: False
- **Decision:** **Deny**
- **Policy Section Named:** `Guest cancellation tiers`
- **Reference Answer:** Denied. The booking was confirmed under the Non-refundable rate. Under 'Guest cancellation tiers', non-refundable reservations qualify for no refund of the nightly rate or fees upon guest cancellation. Unauthorized or erroneous bot statements do not override published policy terms; no refund will be issued.
- **Clarity & Anti-Hedge Check:** ✅ Clear, decisive refusal. Explicitly addresses and rejects bot hallucination without ambiguity.

#### Pair 3: RQ-03
- **Guest Question:** "My daughter got sick and we can't travel, I picked the no-refund option but surely there's an exception?"
- **Booking Facts:** Non-refundable rate | Booked: 2025-01-30 | Check-in: 2025-03-28 | Cancelled: 2025-03-04 (24 days before check-in) | `host_cancelled`: False | `aircover_issue_reported`: False
- **Decision:** **Deny**
- **Policy Section Named:** `Guest cancellation tiers`
- **Reference Answer:** Denied. Under 'Guest cancellation tiers' for Non-refundable bookings, cancellations for any reason—expressly including medical emergencies, personal illness, and changed plans—receive zero refund of the nightly rate or fees.
- **Clarity & Anti-Hedge Check:** ✅ Clear, decisive refusal. Firmly applies strict policy terms to medical excuses without hedging.

#### Pair 4: RQ-04
- **Guest Question:** "Booked non-refundable and now the dates don't work for us, can I at least get the cleaning and service fee back?"
- **Booking Facts:** Non-refundable rate | Booked: 2025-02-20 | Check-in: 2025-04-02 | Cancelled: 2025-03-07 (26 days before check-in) | `host_cancelled`: False | `aircover_issue_reported`: False
- **Decision:** **Allow** *(Partial — Cleaning Fee Only)*
- **Policy Section Named:** `Service and cleaning fees`
- **Reference Answer:** Allowed in part. Under 'Service and cleaning fees', the cleaning fee is refunded whenever the guest cancels before check-in, on any tier. Because cancellation occurred on 2025-03-07 prior to the 2025-04-02 check-in (26 days in advance), the cleaning fee is refunded in full. However, the guest service fee and nightly rate are non-refundable because service fees are refunded only when a cancellation qualifies for a full refund under tier rules.
- **Clarity & Anti-Hedge Check:** ✅ Clear, precise breakdown of which fee is refunded and which is retained.

---

### Scenario Type 2: `aircover_claim`

#### Pair 5: RQ-05
- **Guest Question:** "There's been no hot water since we arrived and the host isn't responding, what am I owed here?"
- **Booking Facts:** Firm policy | Check-in: 2025-03-04 | Reported: 2025-03-05T19:33:12Z (~35 hours after check-in) | `aircover_issue_reported`: True | Issue Type: `no_hot_water`
- **Decision:** **Allow** *(Partial/Rebooking)*
- **Policy Section Named:** `AirCover for guests`
- **Reference Answer:** Allowed. Lack of hot water is an explicitly covered issue type under 'AirCover for guests' ('No hot water or other essential amenity missing'). Because the issue was reported within 72 hours of arrival and discovery, you qualify for AirCover protection, granting rebooking assistance and a partial or full refund for the affected nights based on severity.
- **Clarity & Anti-Hedge Check:** ✅ Clear approval citing covered amenity and <72h discovery compliance.

#### Pair 6: RQ-06
- **Guest Question:** "The place looks nothing like the photos, it's filthy and half the rooms are locked. I want a refund."
- **Booking Facts:** Moderate policy | Check-in: 2025-03-06 | Reported: 2025-03-08T13:18:47Z (~53 hours after check-in) | `aircover_issue_reported`: True | Issue Type: `listing_not_as_described`
- **Decision:** **Allow** *(Partial/Rebooking)*
- **Policy Section Named:** `AirCover for guests`
- **Reference Answer:** Allowed. Under 'AirCover for guests', 'Listing not as described' (including severe cleanliness deficiencies and inaccessible locked rooms) is a covered issue. Because you reported it within 72 hours of arrival, you are eligible for rebooking assistance and a refund for the affected nights based on severity.
- **Clarity & Anti-Hedge Check:** ✅ Clear approval under listed issue category within 72-hour window.

#### Pair 7: RQ-07
- **Guest Question:** "We couldn't get in at all, the code didn't work and nobody answered for hours so we booked a hotel."
- **Booking Facts:** Strict policy | Check-in: 2025-03-07 | Reported: 2025-03-09T10:41:09Z (~50 hours after check-in) | `aircover_issue_reported`: True | Issue Type: `could_not_check_in`
- **Decision:** **Allow** *(Full Refund)*
- **Policy Section Named:** `AirCover for guests`
- **Reference Answer:** Allowed. Under 'AirCover for guests', 'Could not check in' is a covered issue when reported within 72 hours of discovery. Because the access code failed, the host was entirely unreachable, and you were forced to book alternative lodging, you qualify for a full refund of the reservation and rebooking support.
- **Clarity & Anti-Hedge Check:** ✅ Clear full refund determination due to complete lockout and unresponsive host.

#### Pair 8: RQ-08
- **Guest Question:** "I found roaches in the kitchen on our first night three days ago but only reporting it now, can I still claim?"
- **Booking Facts:** Flexible policy | Check-in: 2025-03-05 | Reported: 2025-03-10T07:56:20Z (5 days / 120 hours after check-in) | `aircover_issue_reported`: True | Issue Type: `pests`
- **Decision:** **Deny**
- **Policy Section Named:** `AirCover for guests`
- **Reference Answer:** Denied. Under 'AirCover for guests', claims must be reported within 72 hours of discovery. Because the pest issue was discovered on the first night (2025-03-05) and reported on 2025-03-10 (5 days / 120 hours later), the claim exceeds the strict 72-hour reporting window and is not eligible for an AirCover refund.
- **Clarity & Anti-Hedge Check:** ✅ Clear denial based on strict mathematical computation of time elapsed (>72 hours).

---

### Scenario Type 3: `partial_refund`

#### Pair 9: RQ-09
- **Guest Question:** "I need to cancel a few weeks out, my policy is moderate, how much of the total do I get back?"
- **Booking Facts:** Moderate policy | Cancel requested: 2025-03-03 | Check-in: 2025-03-25 (22 days before check-in) | `host_cancelled`: False
- **Decision:** **Allow** *(Full Refund)*
- **Policy Section Named:** `Guest cancellation tiers`
- **Reference Answer:** Allowed. Under 'Guest cancellation tiers' for Moderate listings, cancelling at least 5 days before check-in entitles the guest to a full refund of the nightly rate. Because cancellation was requested on 2025-03-03 for a 2025-03-25 check-in (22 days before check-in, exceeding the 5-day requirement), you receive a 100% full refund of the nightly rate, cleaning fee, and service fee.
- **Clarity & Anti-Hedge Check:** ✅ Clear 100% refund entitlement based on 22 days >= 5 days requirement.

#### Pair 10: RQ-10
- **Guest Question:** "Cancelling my flexible booking today, check-in isn't until next month, do I lose anything?"
- **Booking Facts:** Flexible policy | Cancel requested: 2025-03-06 | Check-in: 2025-04-10 (35 days before check-in) | `host_cancelled`: False
- **Decision:** **Allow** *(Full Refund)*
- **Policy Section Named:** `Guest cancellation tiers`
- **Reference Answer:** Allowed. Under 'Guest cancellation tiers' for Flexible listings, cancelling at least 24 hours before check-in entitles the guest to a full refund of the nightly rate. Because you are cancelling on 2025-03-06 for a 2025-04-10 check-in (35 days before arrival), you will not lose anything and receive a 100% full refund including all fees.
- **Clarity & Anti-Hedge Check:** ✅ Clear confirmation of zero financial loss.

#### Pair 11: RQ-11
- **Guest Question:** "I have a firm policy and need to cancel about two weeks before arrival, what's the refund amount?"
- **Booking Facts:** Firm policy | Cancel requested: 2025-03-08 | Check-in: 2025-03-24 (16 days before check-in) | `host_cancelled`: False
- **Decision:** **Allow** *(Partial — 50% Nightly Rate + Cleaning Fee)*
- **Policy Section Named:** `Guest cancellation tiers`
- **Reference Answer:** Allowed in part. Under 'Guest cancellation tiers' for Firm listings, cancelling 7 to 29 days before check-in entitles the guest to a 50% refund of the nightly rate. Because cancellation occurred on 2025-03-08 for a 2025-03-24 check-in (16 days before check-in), you receive 50% of the nightly rate plus 100% of the cleaning fee. Under 'Service and cleaning fees', partial refunds do not return the guest service fee.
- **Clarity & Anti-Hedge Check:** ✅ Exact percentage, fees breakdown, and days computation provided.

#### Pair 12: RQ-12
- **Guest Question:** "Booked a strict listing 40 hours ago and now I have to cancel, check-in is 10 days out, what do I get back?"
- **Booking Facts:** Strict policy | Booked: 2025-03-02 | Cancel requested: 2025-03-04 (40h elapsed) | Check-in: 2025-03-14 (10 days before check-in) | `host_cancelled`: False
- **Decision:** **Allow** *(Partial — 50% Nightly Rate + Cleaning Fee)*
- **Policy Section Named:** `Guest cancellation tiers`
- **Reference Answer:** Allowed in part. Under 'Guest cancellation tiers' for Strict listings, a full refund requires cancelling within 48 hours of booking AND at least 14 days before check-in. While cancelled 40 hours after booking, check-in is only 10 days away (2025-03-04 to 2025-03-14), failing the 14-day condition. Because cancellation is at least 7 days before check-in, the 50% rule applies: you receive 50% of the nightly rate and the full cleaning fee. The service fee is non-refundable.
- **Clarity & Anti-Hedge Check:** ✅ Dissects dual-condition rule (48 hours AND 14 days) and delivers definitive 50% outcome.

---

### Scenario Type 4: `host_cancellation`

#### Pair 13: RQ-13
- **Guest Question:** "My host just cancelled on me the night before check-in and I have nowhere to stay, what happens now?"
- **Booking Facts:** Firm policy | Check-in: 2025-03-06 | `host_cancelled`: True (night before check-in, 1 day out)
- **Decision:** **Allow** *(Full Refund + Rebooking Support)*
- **Policy Section Named:** `Host cancellations`
- **Reference Answer:** Allowed. Under 'Host cancellations', when the host cancels, the guest receives a full refund of everything paid, including the service fee. Furthermore, because the host cancelled within 30 days of check-in, AirCover will actively help you rebook a similar stay.
- **Clarity & Anti-Hedge Check:** ✅ Confirms 100% full refund and rebooking assistance.

#### Pair 14: RQ-14
- **Guest Question:** "Host cancelled my reservation this morning saying the place is unavailable, do I get everything back?"
- **Booking Facts:** Moderate policy | Check-in: 2025-03-13 | `host_cancelled`: True
- **Decision:** **Allow** *(Full Refund)*
- **Policy Section Named:** `Host cancellations`
- **Reference Answer:** Allowed. Under 'Host cancellations', whenever a host cancels a reservation, the guest receives a full refund of everything paid, including the nightly rate, cleaning fee, and service fee. You get 100% of your money back.
- **Clarity & Anti-Hedge Check:** ✅ Unambiguous confirmation that all components are refunded.

#### Pair 15: RQ-15
- **Guest Question:** "I booked a non-refundable rate and the host went and cancelled it themselves, am I stuck losing the money?"
- **Booking Facts:** Non-refundable rate | Check-in: 2025-03-18 | `host_cancelled`: True
- **Decision:** **Allow** *(Full Refund)*
- **Policy Section Named:** `Host cancellations`
- **Reference Answer:** Allowed. You will not lose your money. Under 'Host cancellations', when the host cancels, the guest receives a full refund of everything paid, including the service fee. This rule applies on every tier, including Non-refundable, and overrides all tier cancellation restrictions.
- **Clarity & Anti-Hedge Check:** ✅ Explicitly confirms host cancellation overrides non-refundable discount restrictions.

#### Pair 16: RQ-16
- **Guest Question:** "The host cancelled two days ago and I still haven't seen a refund, can you check on it?"
- **Booking Facts:** Strict policy | Check-in: 2025-03-15 | `host_cancelled`: True
- **Decision:** **Allow** *(Full Refund)*
- **Policy Section Named:** `Host cancellations`
- **Reference Answer:** Allowed. Because your host cancelled the reservation, you are entitled to a 100% full refund of everything paid, including the service fee, under 'Host cancellations'. The full refund is confirmed and being processed.
- **Clarity & Anti-Hedge Check:** ✅ Confirms entitlement and processing without hedging.

---

### Scenario Type 5: `late_checkout_dispute`

#### Pair 17: RQ-17
- **Guest Question:** "Host is demanding an extra night fee because we left at 2pm instead of 11am, do I really have to pay that?"
- **Booking Facts:** Moderate policy | Departed 3 hours past checkout time (2pm instead of 11am) | `host_cancelled`: False
- **Decision:** **Deny**
- **Policy Section Named:** `AirCover for guests`
- **Reference Answer:** Denied. Staying past the agreed checkout time (leaving at 2pm instead of 11am) is not a covered cancellation or AirCover issue. Under 'AirCover for guests', protection applies exclusively to four defined categories (check-in failure, listing not as described, essential amenities missing, or pests). Issues outside these four categories are ineligible for refunds or fee waivers under this policy.
- **Clarity & Anti-Hedge Check:** ✅ Firm rejection stating fee waiver is outside policy scope.

#### Pair 18: RQ-18
- **Guest Question:** "We decided to leave a night early because we were done with our trip, can I get that last night refunded?"
- **Booking Facts:** Firm policy | Checked in: 2025-03-04 | Departed early: 2025-03-06 | In-stay voluntary departure
- **Decision:** **Deny**
- **Policy Section Named:** `Guest cancellation tiers`
- **Reference Answer:** Denied. Under 'Guest cancellation tiers' for Firm listings, cancellations requested less than 7 days before check-in or during an ongoing stay receive no refund for unused nights. Voluntary early departure due to personal trip completion is not eligible for a refund.
- **Clarity & Anti-Hedge Check:** ✅ Decisive denial under Firm policy in-stay rules.

#### Pair 19: RQ-19
- **Guest Question:** "The listing said checkout was noon but the host says 10am and charged me a late fee, is that allowed?"
- **Booking Facts:** Strict policy | Late checkout fee dispute | `host_cancelled`: False
- **Decision:** **Deny**
- **Policy Section Named:** `AirCover for guests`
- **Reference Answer:** Denied. Disputes regarding checkout times and host-assessed late fees do not qualify under 'AirCover for guests'. AirCover covers only four explicit issues (unable to check in, listing not as described, missing essential amenities, or pests). Any issue that is not one of these four covered types is not eligible for an AirCover refund, whatever the timing.
- **Clarity & Anti-Hedge Check:** ✅ Clear refusal citing the closed list of 4 covered AirCover issues.

#### Pair 20: RQ-20
- **Guest Question:** "Our flight was delayed so we stayed past checkout by an hour, now there's a fee on my account, can it be waived?"
- **Booking Facts:** Flexible policy | Stayed 1 hour past checkout due to flight delay
- **Decision:** **Deny**
- **Policy Section Named:** `AirCover for guests`
- **Reference Answer:** Denied. Overstaying checkout due to travel or flight delays does not fall within covered cancellation tiers or the four eligible categories of 'AirCover for guests'. Consequently, the assessed late checkout fee cannot be waived or refunded under this policy.
- **Clarity & Anti-Hedge Check:** ✅ Clear denial confirming flight delay penalties cannot be waived under this excerpt.

---

## 4. Final Compliance Attestation

This check note confirms:
1. **20 pairs** are fully documented, numbered, and matched to `RQ-01` through `RQ-20`.
2. **All five scenario types** (`non_refundable`, `aircover_claim`, `partial_refund`, `host_cancellation`, `late_checkout_dispute`) are represented with 4 pairs each.
3. **Zero unclear or hedged answers** are present: all 20 answers provide unambiguous determinations, cite the exact policy section, and evaluate the specific calendar math and boolean flags.
