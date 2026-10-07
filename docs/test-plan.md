# Test Plan (pre-registered)

**The rule of this file:** every hypothesis and pass/fail threshold below is written down **before** the build. Once the hackathon starts, this file is **never edited**. If we need to change a rule, we add a dated amendment at the bottom, written before we look at the data it governs. Git history proves the order.

Why bother? A hackathon project can make almost any chart look good. Fixing the rules first means a judge can trust our results, including the ones that fail.

---

## Shared definitions

| Term | Meaning |
|---|---|
| **Cell-day** | One 0.01° grid cell with ≥ 1 detection on one day, per sensor and per pass (day / night) |
| **Rate** | Cell-days ÷ cloud-free cells × 1,000 |
| **Aqua-equivalent (MYD-eq)** | A rate converted to what Aqua MODIS would have recorded |
| **Season** | 1 July → 30 June, so one dry season is never split across two years |
| **Dry / firing months** | November → May |
| **Monsoon months** | June → October (kilns shut; our "nothing should happen here" baseline) |
| **Kiln cluster** | Mapped kilns grouped so no cluster is smaller than a VIIRS pixel's position error |
| **Matched control** | 3 farmland sites per cluster: same district, same ESA WorldCover class, 5–10 km from any kiln, same footprint shape |
| **Confidence filter** | Nominal + high, for every sensor |

---

## Part A: Harmonization

### T1. Does the fake 2012 jump disappear?

- **Hypothesis.** The raw MODIS+VIIRS record shows a break at the 2012 sensor change. After harmonization, it doesn't.
- **Measure.** *d* = mean of the three seasons after the switch minus the three seasons before, for the raw and the harmonized national series. Chow break test at the switch.
- **Pass if all hold:**
  - `d_harmonized ≤ 0.25 × d_raw`
  - Chow test on raw: p < 0.01
  - Chow test on harmonized: p > 0.05
- **If it fails:** we publish the remaining jump and try the pre-declared backup model (T2b), never a new one made up after seeing the data.

### T2. Are our error ranges honest? (leave-one-season-out)

- **Method.** For each overlap season: learn the conversion without it, predict it, check whether the true value falls inside our 95% range.
- **Pass if:** pooled coverage over all held-out values is between **0.90 and 0.97**.
  - Below 0.90 → ranges too narrow (over-confident). We widen them and say so.
  - Above 0.97 → ranges too wide (over-cautious). We report it as a conservative failure.
- **T2b, model choice (pre-declared).** M0 = a simple ratio per stratum (region × month × day/night). M1 = a Poisson regression with month, region and scan-angle terms. **M1 ships only if its leave-one-season-out error is more than 10% lower than M0's.** Otherwise the simpler model wins.

### T3. Do we agree with a sensor that never changed?

- **Method.** Aqua MODIS kept flying through 2012–2021. Compare our VIIRS-based, converted values with what Aqua actually saw in those years.
- **Pass if:** Aqua's observed value falls inside our 95% range in at least 8 of 10 seasons.
- **Note.** Aqua's orbit drifted from 2022, so drift-era Aqua is flagged and never used for calibration.

### T6. Does the bridge survive the end of S-NPP?

- **Method.** Calibrate the S-NPP → NOAA-20 → NOAA-21 links on their overlaps. Hold out the most recent complete months and predict them.
- **Pass if:** held-out coverage of the 95% range is ≥ 0.85 (smaller sample, so a looser bar, declared now).

---

## Part B: The kiln question

### T4. Can NASA fire satellites see brick kilns at all? (gate)

- **Hypothesis (genuinely open).** A firing kiln is a small, very hot spot inside a 375 m pixel. Its total heat *might* cross the VIIRS detection threshold, especially at night. It might not: kilns burn inside closed chambers. No published method detects kilns from fire data, so we don't know.
- **Test season.** The latest complete dry season inside the kiln inventory's imagery window, chosen by that rule only, never by looking at fire data.

Statistics per cluster and per control:

| | Meaning |
|---|---|
| **DR** | Detection rate: days with a detection ÷ cloud-free days |
| **S** | Season coverage: share of the 30 dry-season weeks with a detection (kiln-like = a long plateau) |
| **P** | Peak concentration: share of detections in the best 14 days (crop-like = a short spike) |

**G1 (VIIRS) passes only if all three hold:**

1. **Shape:** median S(kiln) ≥ 2 × median S(control) **and** median P(kiln) < median P(control); one-sided Mann–Whitney p < 0.01 for both.
2. **Contrast:** mean DR(kiln) ÷ mean DR(control) ≥ 3; permutation test within districts (10,000 shuffles), p < 0.01.
3. **Seasonality:** DR(kiln, Nov–May) ≥ 3 × DR(kiln, Jun–Oct).

**G2 (MODIS):** the same three rules. Decides whether any kiln history before 2012 is possible.

**What we build in each outcome:**

| Outcome | What happens |
|---|---|
| G1 pass, G2 pass | Kiln/crop split from 2003; kiln calendar 2003 → today |
| G1 pass, G2 fail | Kiln/crop split and kiln calendar from 2012 |
| G1 fail | The fire calendar is declared kiln-free (nothing to remove), we publish the negative result, and run **T5** |

### T5. If fire sensors miss kilns, can other sensors see kiln *activity*?

Runs only if G1 fails. Candidate channels, each with a clear physical reason:

| Channel | Why it might work |
|---|---|
| **NASA Black Marble VNP46A2** night lights | Kilns are worked through the night |
| **Sentinel-1 radar** (yard minus surrounding ring) | Stacks of fresh bricks and moved earth change the radar return |
| **ECOSTRESS** night surface temperature (70 m) | Sharper thermal pixels |
| **Landsat 8/9** surface temperature (day) | Sharper thermal pixels |
| **TROPOMI** NO₂ / SO₂ | Kiln smoke |

**Procedure (to avoid fooling ourselves):**

1. **Pilot** on a small, randomly drawn set of clusters (fixed random seed, declared in an amendment). Every pilot result is published, including the failures.
2. **Only channels that show a pilot signal go forward.** Their exact confirmatory rules are written as a dated amendment **before** touching the confirmatory data.
3. **Confirm** on all *other* clusters (never the pilot ones), in a season no pilot used.

**Default confirmatory rule (to be refined only by amendment):**

- Per cluster: seasonal amplitude A = mean(kiln − controls) in Dec–Apr minus the same in Jul–Oct.
- **Pass if:** median A > 0 with one-sided p < 0.001, more than 60% of clusters have A > 0, the sign holds in at least 80% of seasons, and a **placebo test** (controls vs other controls) shows nothing (p > 0.05).

---

## Part C: People

### U1. Can ordinary people use it?

- **Who:** at least 5 adult volunteers in Mymensingh (for example a teacher, a student over 18, a farmer's family member, a local journalist, an NGO worker). Written consent. **No one under 18**, and no faces or voices recorded without consent, as the rules require.
- **Tasks:**
  1. "Find when burning season usually happens in your upazila."
  2. "Is this year more or less smoky than normal so far?"
  3. "Print the Agun Ponjika for your upazila."
- **Pass if:** ≥ 80% task success, median time for task 1 under 60 seconds, and at least 4 of 5 testers can explain in their own words what the calendar shows.
- **We record:** where people got stuck, which words confused them, Bangla vs English preference.

---

## Part D: Engineering checks

| Check | What it guarantees |
|---|---|
| **Unit tests on synthetic data only** | Tests never depend on downloaded data, so anyone can run them |
| **Grid parity test** | Python and the website put the same point in the same cell |
| **Data contract test** | The website can read every file the pipeline writes, in every gate outcome |
| **Public-safety test** | No kiln coordinate, kiln ID or cluster ID ever appears in public files; enforced in CI |
| **Smoke test** | Every page of the built site loads |
| **Clean-clone test** | A fresh clone reproduces the headline numbers |

---

## Reporting promise

Every test above gets a line in the final results table: **PASS**, **FAIL**, or **NOT RUN (why)**. Failed tests stay visible on the website's "For experts" page.

---

## Amendments

*None yet. Amendments are added below this line, dated, before the data they govern is analysed.*
