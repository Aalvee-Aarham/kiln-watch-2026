# Hackathon Plan: 13–14 November 2026

Two days, five roles, one rule: **the harmonized calendar ships no matter what.** Everything else is layered on top.

## Before the event (allowed: preparation, not building)

- [ ] Accounts: NASA Earthdata, FIRMS MAP_KEY, Google Earth Engine, GitHub
- [ ] Datasets downloaded and cached offline (FIRMS archive for Bangladesh, boundaries, kiln inventory)
- [ ] Laptops set up: Python 3.11, Node 20, Git; environment tested at home
- [ ] Test plan committed ([`test-plan.md`](test-plan.md)) so its rules pre-date the build
- [ ] Slide template and demo script started
- [ ] 28 Oct: read the full challenge statement; adjust the challenge table in [`proposal.md`](proposal.md)
- [ ] U1 volunteers in Mymensingh lined up (adults, consent forms ready)
- [ ] Chargers, extension cord, power bank; sleep, food and water planned

## Roles

| Role | Owns |
|---|---|
| **Pipeline lead** | Ingest, grid, cloud-free counts, export |
| **Science lead** | Harmonization, tests T1–T6, error ranges |
| **Web lead** | Website, map, calendar, daily update |
| **Design + people lead** | Bangla content, Agun Ponjika, user test U1, accessibility |
| **Story lead** | Videos 2 and 3, NASA project page, slides |

## Hour-by-hour (aligned with the Bangladesh run of show)

### Day 1: Friday 13 November

| Time | Pipeline + science | Web + design | Story |
|---|---|---|---|
| 07:00–10:30 | Registration, mentor meet, NASA site check | ← | ← |
| 10:30–12:30 | Ingest FIRMS + boundaries; grid | App skeleton, map, area search | Team page on NASA site; roles; video plan |
| 12:30–15:00 | Lunch / prayer · cloud-free counts | Calendar component on sample data | Script Video 2 |
| 15:00–18:00 | Harmonize; run T1–T3 | Real data into "My area" | Record screen captures |
| **18:00–18:30** | **Freeze a demo build** | ← | **Video 2 edit (≤ 240 s, demo + NASA data named on camera)** |
| **18:30** | | | **Upload Video 2 (YouTube + Drive folder)** |
| 19:00–23:00 | Kiln clusters + controls; T4 gate | Sensor-switch page; Bangla labels | NASA page draft |
| 23:00 → | T5 pilot if G1 fails; T6 | Agun Ponjika print view | 30-second script |

### Day 2: Saturday 14 November

| Time | Pipeline + science | Web + design | Story |
|---|---|---|---|
| 07:30–09:00 | Results table (PASS / FAIL / NOT RUN) | U1 remote check with 2–3 volunteers; fix the worst issue | Record Video 3 |
| **09:00–12:00** | Final export; deploy | Smoke test; private-window check | **Video 3 (30 s, English subtitles) uploaded by 12:00 PM** |
| 10:00–11:00 | Judges and mentors round | ← | ← |
| 11:00 → | Local judging: 240 Seconds of Glory | ← | ← |
| By event close | README results table; NASA page **submitted** | Repository public, tested logged-out | Use of AI and references fields complete |

## Scope tiers

| Tier | What | Rule |
|---|---|---|
| **Must** | Harmonized calendar for Mymensingh division + national view; T1–T3; T4 gate result; My area page; method page; public-safety test | Ships even if everything else fails |
| **Should** | All 64 districts and upazilas; T5 pilot; T6; Sensor switch; daily update; Bangla interface | Start only when Must is green |
| **Could** | Agun Ponjika print view; drawn-box areas; Timeline page; U1 full study | Nice to have |

## Risks and backups

| Risk | Backup |
|---|---|
| Venue internet is slow | All data cached offline before the event |
| Earth Engine quota or timeouts | Pre-cache cloud-free counts for Mymensingh division first; national later |
| FIRMS API limits | Archive download first; API only for the latest days |
| Harmonization test fails | Publish it, apply the pre-declared backup model (T2b) only |
| Kilns invisible to fire sensors | Expected possibility; publish it and run T5 |
| Running out of time | Cut from **Could**, then **Should**. Never cut a test result |
| Video upload fails | Upload from two laptops / phone hotspot; deadline buffer of 30 min |
