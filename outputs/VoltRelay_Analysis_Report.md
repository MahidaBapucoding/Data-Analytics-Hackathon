# VoltRelay Energy — Analysis Report

## 1. Executive Summary
- Latest observed month (2025-06): 303,196 completed swaps from 331,719 attempts; failure rate 5.6%; charged revenue ₹19,690,790; estimated energy-only contribution proxy per completed swap ₹48.
- Observed 30-day repeat rate among riders with at least 30 days of follow-up: 99.6% (eligible n=18,693).
- City failure rates ranged from 5.1% in Jaipur (n=487,634) to 2.7% in Mumbai (n=530,299); descriptive association only.
- In same-pack, same-age 2W_2.1kWh cohort, delivered SOH averaged 80.5% for Kyron vs 89.5% for Cellora; observational, not supplier causation.
- The widest first-experience 30-day-return gap among groups with n≥100 was 1.7pp for first_station_id; unadjusted association.

## 2. Problem Understanding
Assess growth, service quality, estimated transaction economics and rider repeat behavior using the supplied observational data.

## 3. Dataset & Methodology
- swap_events.csv: 3,877,013 rows
- station_hourly_status.csv: 1,487,712 rows
- riders.csv: 20,000 rows
- batteries.csv: 6,500 rows
- support_tickets.csv: 44,000 rows
- stations.csv: 152 rows
- city_daily_context.csv: 3,282 rows
- fleet_partners.csv: 12 rows

Swap events were processed in chunks; the primary event output retains near-duplicate candidates. Joins use unique station/rider/battery keys where validated.

## 4. Data Quality
- rows_read: 3,877,013
- bad_timestamp: 0
- negative_distance: 7,814
- large_distance_gt_300km: 7,712
- soc_over_100: 3,698
- soh_over_100: 6,155
- offline_duplicate_candidates: 0
- unmatched_station: 0
- firmware_timestamp_corrected: 141,280

City aliases are standardized case-insensitively (mapping shown in the notebook); test stations are excluded from event KPIs. Duplicate candidates are limited to the implemented within-chunk exact rider/station/battery/event match and are retained. Distance values below zero and above 300km are flagged and retained; 300km is a conservative plausibility threshold for distance since one swap, not a row-deletion rule. Percentages above 100 are counted; bounded display clips at 100 only where explicitly labeled. Missing telemetry is not treated as zero. CSAT coverage is reported with tickets and is not described as representative.

## 5. Network Performance
- Latest observed month (2025-06): 303,196 completed swaps from 331,719 attempts; failure rate 5.6%; charged revenue ₹19,690,790; estimated energy-only contribution proxy per completed swap ₹48.
- Highest monthly failure rate in the observed analysis window was 8.1% in 2024-05 (11,629/143,189 failed attempts). This is descriptive and not causal.
- Observed 30-day repeat rate among riders with at least 30 days of follow-up: 99.6% (eligible n=18,693).

## 6. Service Failure Analysis
- Across city-tagged events, Jaipur had the highest failure rate at 5.1% (24,677/487,634); Mumbai had the lowest at 2.7% (14,441/530,299). Descriptive city differences do not isolate operating causes.
- The highest hourly failure rate was at 20:00: 4.2% (19,259/454,428 attempts), after the stated firmware timestamp correction.
- Among stations with at least 10,000 attempts, STN-DEL-037 had the highest failure rate (5.9%, 1,823/30,838); configured capacity was 16 slots and mean recorded queue wait was 243 seconds.
- For 256,610 high-demand station-hours vs 256,610 low-demand station-hours, failure rates were 3.9% vs 3.6%; high demand is defined as attempts per configured slot-hour quartile.
- Observed failure rate was 4.3% in the lowest charged-stock/target quartile (n=434,080 attempts) and 3.5% in the highest quartile (n=389,238); the comparison includes 464,818 of 1,026,438 ok/partial event-hours with calculable stock. Telemetry-missing and stock-unavailable hours are excluded.

## 7. Station & Geographic Analysis
- Across city-tagged events, Jaipur had the highest failure rate at 5.1% (24,677/487,634); Mumbai had the lowest at 2.7% (14,441/530,299). Descriptive city differences do not isolate operating causes.
- Among stations with at least 10,000 attempts, STN-DEL-037 had the highest failure rate (5.9%, 1,823/30,838); configured capacity was 16 slots and mean recorded queue wait was 243 seconds.
- Among stations with at least 100 matched telemetry hours and 10,000 attempts, STN-DEL-037 had 5.9% failure rate over 30,838 attempts, mean 0.21 attempts per configured slot-hour, charged-stock/target ratio unavailable, and 242s mean queue.

Expansion-wave comparisons are unadjusted and do not estimate an expansion effect.

## 8. Battery Analysis
- Within 2W_2.1kWh batteries in age band 6-12mo, mean delivered SOH ranged from 80.5% (Kyron, 556,365 swaps) to 89.5% (Cellora, 431,707 swaps). This is a cohort association; it does not establish supplier causation.

## 9. Pricing & Fleet Economics
- Across 3,586,675 completed swaps from 3,814,982 in-scope attempts, observed charges were ₹229,195,296 (₹64 average charge and revenue per completed swap). Recorded discounts were ₹20,875,332 (8.5% of completed-swap list price; 2,368,154 completed swaps had a positive discount).
| Tariff | Attempts | Completed | Charged revenue | Avg. charge / completion | Recorded discounts | Discount / list | Est. energy-only proxy / completion |
|---|---:|---:|---:|---:|---:|---:|---:|
| OFFPEAK | 36,388 | 34,833 | ₹2,071,886 | ₹59 | ₹0 | 0.0% | ₹42 |
| PARTNER | 2,521,737 | 2,368,154 | ₹146,079,774 | ₹62 | ₹20,875,332 | 12.6% | ₹42 |
| PEAK | 171,696 | 163,922 | ₹13,523,775 | ₹83 | ₹0 | 0.0% | ₹65 |
| STD | 1,085,161 | 1,019,766 | ₹67,519,860 | ₹66 | ₹0 | 0.0% | ₹48 |
- Validated assignment to the 12 unique fleet partners covered 2,521,737 attempts and 2,368,154 completions, with ₹146,079,774 charged revenue and ₹20,875,333 recorded discount exposure. ZipDrop had the most completed swaps (549,390); FeastFly had the highest charged revenue (₹32,027,884); ZipDrop had the largest recorded discount exposure (₹8,033,300).
| Fleet partner | Attempts | Completed | Charged revenue | Recorded discount exposure | Realized discount / list | Contract rate at 2025-06-30 | Avg. charge / completion |
|---|---:|---:|---:|---:|---:|---:|---:|
| Unassigned rider partner | 1,293,245 | 1,218,521 | ₹83,115,521 | ₹0 | 0.0% | — | ₹68 |
| FeastFly | 549,133 | 516,367 | ₹32,027,884 | ₹3,429,956 | 10.0% | 10.0% | ₹62 |
| ZipDrop | 583,937 | 549,390 | ₹28,606,515 | ₹8,033,300 | 21.9% | 28.0% | ₹52 |
| ParcelNest | 312,702 | 296,209 | ₹16,740,213 | ₹2,945,483 | 15.0% | 15.0% | ₹57 |
| Swiggle Go | 252,273 | 237,632 | ₹14,092,429 | ₹1,741,741 | 11.0% | 11.0% | ₹59 |
| CargoTuk | 146,011 | 131,334 | ₹13,062,631 | ₹1,134,503 | 8.0% | 8.0% | ₹99 |
| QuickCart | 135,718 | 127,961 | ₹7,934,509 | ₹757,764 | 9.0% | 9.0% | ₹62 |
| RideMitra | 126,484 | 119,503 | ₹7,494,596 | ₹473,170 | 6.0% | 6.0% | ₹63 |
| DabbaXpress | 97,697 | 92,027 | ₹5,853,724 | ₹489,558 | 8.0% | 8.0% | ₹64 |
| HaulKing | 59,159 | 53,120 | ₹5,331,093 | ₹400,956 | 7.0% | 7.0% | ₹100 |
| UrbanErrand | 87,144 | 82,363 | ₹5,226,978 | ₹272,192 | 5.0% | 5.0% | ₹63 |
| LastMileHub | 85,004 | 80,332 | ₹4,878,057 | ₹540,236 | 10.0% | 10.0% | ₹61 |
| MetroMove | 86,475 | 81,916 | ₹4,831,146 | ₹656,473 | 12.0% | 12.0% | ₹59 |
- Estimated contribution proxy: charged revenue less the existing estimated recharge-electricity cost was ₹159,277,534 total, or ₹44 per completed swap. This is an energy-only contribution proxy, not contribution margin.
- Reconciliation: recorded completed-swap charges exceeded list price less recorded discounts by ₹4,074,333; the supplied transaction fields do not fully explain this pricing adjustment. Actual charged amounts are used as revenue.

No contribution margin is claimed. The energy-only proxy excludes rent, maintenance, labor, fees, battery depreciation and other costs; partner assignments use the rider profile snapshot.

## 10. Rider Retention
- Among city × vehicle × plan groups with at least 100 eligible riders, 30-day return ranged from 98.0% (n=101; Bengaluru, 3W, pay_as_you_go) to 100.0% (n=394; Delhi NCR, 3W, partner_billed).
- First-experience comparison with the widest observed 30-day-return gap (group size ≥100) was first_station_id: 98.3% for STN-BLR-011 (n=175) vs 100.0% for STN-BLR-001 (n=165), an absolute difference of 1.7 percentage points. Unadjusted association; multiple comparisons are descriptive.
- Thirty-day return was 99.0% among riders whose first attempt failed (n=1,024) and 99.6% among those without a first-attempt failure (n=17,669), an unadjusted difference of 0.6 percentage points. Return was high in both groups.

First completed experience includes queue, station/city/type, tariff, delivered SOH, first attempt outcome and ticket within 7 days. Group comparisons require at least 100 eligible riders and are unadjusted.

## 11. Cross-Domain Root Cause Synthesis
These patterns are observational associations; first-experience comparisons are not adjusted for confounding and do not establish causes.

## 12. Recommendations
- **Evidence:** Across city-tagged events, Jaipur had the highest failure rate at 5.1% (24,677/487,634); Mumbai had the lowest at 2.7% (14,441/530,299). Descriptive city differences do not isolate operating causes.  
**Business problem:** City-level failures exceed the network-wide observed rate.  
**Action:** Review only stations in the higher-rate city and compare their queue, charged inventory and outage conditions before changing capacity.  
**KPI to monitor:** Station-hour failure rate, queue wait, charged-stock/target ratio.
- **Evidence:** Among stations with at least 10,000 attempts, STN-DEL-037 had the highest failure rate (5.9%, 1,823/30,838); configured capacity was 16 slots and mean recorded queue wait was 243 seconds.  
**Business problem:** A high-volume station has the largest observed failure rate among stations meeting the 10,000-attempt threshold.  
**Action:** Inspect its matched telemetry and operating hours; validate local demand and stock before a station-specific capacity or scheduling pilot.  
**KPI to monitor:** Station-hour failure rate, queue wait, outage minutes, charged-stock/target ratio.
- **Evidence:** For 256,610 high-demand station-hours vs 256,610 low-demand station-hours, failure rates were 3.9% vs 3.6%; high demand is defined as attempts per configured slot-hour quartile.  
**Business problem:** Failure rate is higher in the normalized high-demand quartile.  
**Action:** Review only top-quartile station-hours and test a local operating adjustment after checking inventory and outage context.  
**KPI to monitor:** Failure rate, mean queue wait, charged-stock/target ratio by attempts-per-slot quartile.
- **Evidence:** Observed failure rate was 4.3% in the lowest charged-stock/target quartile (n=434,080 attempts) and 3.5% in the highest quartile (n=389,238); the comparison includes 464,818 of 1,026,438 ok/partial event-hours with calculable stock. Telemetry-missing and stock-unavailable hours are excluded.  
**Business problem:** Low charged-stock station-hours have a higher observed failure rate.  
**Action:** Inspect the affected stations and validate inventory timing before any targeted stock change; do not generalize across the network.  
**KPI to monitor:** Failure rate, queue wait and charged-stock/target ratio for matched station-hours.
- **Evidence:** Within 2W_2.1kWh batteries in age band 6-12mo, mean delivered SOH ranged from 80.5% (Kyron, 556,365 swaps) to 89.5% (Cellora, 431,707 swaps). This is a cohort association; it does not establish supplier causation.  
**Business problem:** One same-pack, same-age supplier cohort has lower observed delivered SOH.  
**Action:** Audit the cohort’s SOH readings and manufacturing lots, then verify against usage and maintenance records before any supplier action.  
**KPI to monitor:** Delivered SOH by supplier × pack × age, battery age, usage and battery-related failure rate.

## 13. Limitations
Energy contribution is an estimated transaction proxy, excluding fixed station costs, labor, fees, battery depreciation and other accounting costs. Event aggregates are joined one-to-one to station-hour telemetry after timestamp correction (1,033,162 event-hours; 0 duplicate keys). The key exists for each event-hour; 6,724 joined hours have telemetry_status=missing. Of 1,026,438 joined hours marked ok/partial, 464,818 have a calculable charged-stock/target ratio. Telemetry-missing and stock-unavailable hours are excluded from stock quartiles. This ratio uses observed minimum stock divided by configured inventory target and does not account for vehicle mix. Relationships are observational. Duplicate candidates may cross processing chunk boundaries. Support categories are agent-assigned. CSAT is non-randomly missing. Retention uses only the first subsequent completed event and full follow-up eligibility; cohort differences remain unadjusted.

## 14. Conclusion
All numeric claims are generated from calculations in this notebook. Core analyses that are not joined or modeled are labeled as unresolved rather than filled with assumed explanations.
