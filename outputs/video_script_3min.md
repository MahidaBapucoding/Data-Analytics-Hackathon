# 3-minute video script

## 0:00–0:25 — Business problem
VoltRelay needs to understand whether growth aligns with reliable service, returning riders and viable per-swap economics.

## 0:25–0:55 — Analytical approach
We streamed the event log, corrected the specified firmware timestamp window, normalized rider city labels, excluded internal test stations from network KPIs, and defined an energy-only transaction contribution proxy. Completed swaps billed ₹229,195,296 total, averaging ₹64 per completion; the estimated energy-only proxy was ₹44 per completion, not a margin.

## 0:55–2:10 — Most important findings
- Observed failure rate was 4.3% in the lowest charged-stock/target quartile (n=434,080 attempts) and 3.5% in the highest quartile (n=389,238); the comparison includes 464,818 of 1,026,438 ok/partial event-hours with calculable stock. Telemetry-missing and stock-unavailable hours are excluded.
- Within 2W_2.1kWh batteries in age band 6-12mo, mean delivered SOH ranged from 80.5% (Kyron, 556,365 swaps) to 89.5% (Cellora, 431,707 swaps). This is a cohort association; it does not establish supplier causation.
- Thirty-day return was 99.0% among riders whose first attempt failed (n=1,024) and 99.6% among those without a first-attempt failure (n=17,669), an unadjusted difference of 0.6 percentage points. Return was high in both groups.

## 2:10–2:50 — Recommendations
- Verify local stock telemetry and review demand, queue and outages before piloting any inventory change; monitor failure rate and stock coverage.
- Audit the matched SOH cohort readings, lots and usage before supplier action; monitor delivered SOH and battery-related failures.
- Review first-attempt failures in adjusted cohorts before service-policy changes; monitor eligible 30-day return.

## 2:50–3:00 — Conclusion
These results describe associations in the supplied data. They do not prove causes; validate any intervention prospectively.
