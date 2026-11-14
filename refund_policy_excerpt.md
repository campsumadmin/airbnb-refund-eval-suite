# Cancellation, Refund and AirCover Policy Excerpt

Policy in effect as of 1 January 2025, for United States listings. All amounts in USD. Applied to the booking facts in `airbnb_guest_refund_questions_mar2025.csv`.

## Guest cancellation tiers

The listing's cancellation policy (the `listing_cancellation_policy` field) sets the refund window for a guest-initiated cancellation. Count the days between the cancellation date and the check-in date to find which rule applies.

| Tier | Condition | Outcome |
|------|-----------|---------|
| Flexible | Cancelled at least 24 hours before check-in | Full refund of the nightly rate |
| Flexible | Cancelled less than 24 hours before check-in | No refund for the first night; full refund for the unused nights after that |
| Moderate | Cancelled at least 5 days before check-in | Full refund of the nightly rate |
| Moderate | Cancelled less than 5 days before check-in | 50% of the nightly rate for the unused nights |
| Firm | Cancelled at least 30 days before check-in | Full refund of the nightly rate |
| Firm | Cancelled 7 to 29 days before check-in | 50% of the nightly rate |
| Firm | Cancelled less than 7 days before check-in | No refund |
| Strict | Cancelled within 48 hours of booking AND at least 14 days before check-in | Full refund of the nightly rate |
| Strict | Otherwise, cancelled at least 7 days before check-in | 50% of the nightly rate |
| Strict | Cancelled less than 7 days before check-in | No refund |
| Non-refundable | Guest took the 10% discount at booking, then cancels for any reason, including changed travel plans | No refund of the nightly rate or any fee |

## Service and cleaning fees

The guest service fee is refunded only when the cancellation qualifies for a **full** refund under the tier rules above. A partial (50% or first-night) outcome does not return the service fee. The cleaning fee is refunded whenever the guest cancels before check-in, on any tier.

## Host cancellations

When the host cancels (the `host_cancelled` field is true), the guest receives a full refund of everything paid, including the service fee. This applies on every tier, including Non-refundable, and overrides the tier windows above. If the host cancels within 30 days of check-in, AirCover also helps the guest rebook a similar stay.

## AirCover for guests

AirCover is Airbnb's guest protection for problems with a stay. It applies only to the covered issue types below, and only when the guest reported the issue to Airbnb within 72 hours of discovering it (`aircover_issue_reported` and `aircover_issue_type`).

| Covered issue type | Outcome when reported within 72 hours |
|--------------------|----------------------------------------|
| Could not check in | Rebooking help, or a full or partial refund for the affected nights, based on severity |
| Listing not as described | Rebooking help, or a full or partial refund for the affected nights, based on severity |
| No hot water or other essential amenity missing | Rebooking help, or a full or partial refund for the affected nights, based on severity |
| Pests | Rebooking help, or a full or partial refund for the affected nights, based on severity |

An issue reported more than 72 hours after it was discovered is not eligible for an AirCover refund. An issue that is not one of the four covered types is not eligible for an AirCover refund, whatever the timing.

*AirCover section reviewed by Leila Haddad, AirCover Policy Analyst.*