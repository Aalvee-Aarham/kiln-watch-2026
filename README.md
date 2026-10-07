# Kiln Watch

A consistent fire calendar for Bangladesh, built from 20+ years of NASA satellite data.

NASA Space Apps Challenge 2026 · Mymensingh, Bangladesh
Challenge: *Harmonization of MODIS and VIIRS Hot Spots*

This repository contains our concept and plan. Space Apps rules don't allow building before the event, so we will write the code during the hackathon on 13–14 November 2026.

---

## The problem

Bangladesh has two kinds of burning in the dry season. After the rice harvests, farmers burn leftover straw in the fields. Brick kilns run at the same time, from about November to April, and they are one of the main causes of winter air pollution.

NASA satellites have recorded fires over Bangladesh since 2003, but the record isn't consistent. Until 2012 the main fire sensor was MODIS, which sees the ground in 1 km squares. Then VIIRS arrived. It sees 375 m squares and picks up many small fires that MODIS misses. If you put the two records together, fires seem to jump sharply in 2012. That jump comes from the new sensor, not from more burning.

This matters now. The first VIIRS satellite (S-NPP) stops sending data in November 2026, and MODIS is expected to shut down in 2027. If the two records can't be joined properly, the long-term fire history becomes hard to use.

There is also an open question: do fire satellites see brick kilns at all? If they do, kiln heat could be mixed into what looks like crop burning. Nobody has tested this.

## Our approach

**1. Put all sensors on one scale.** We place every fire detection on a 1 km grid and account for cloud cover, so a cloudy day is never counted as a day with no fire. Then we convert every sensor's numbers to match Aqua MODIS. We learn the conversion from the years when two satellites were flying at the same time: Aqua to S-NPP, S-NPP to NOAA-20, NOAA-20 to NOAA-21. With that chain, the record can keep going after the older satellites stop. Every value comes with an error range.

**2. Test whether kilns show up in the fire data.** We compare fire detections at mapped brick kilns with nearby farmland that has the same land cover. If kilns do appear, we separate kiln heat from crop fires. If they don't, the fire calendar is already clean, and we test other NASA data that might show when kilns are working, such as Black Marble night lights.

**3. Make it useful to people.** We are building a simple website, in Bangla and English, where anyone can pick a district or upazila. It shows when burning season usually happens there, whether this year is unusual, and how things have changed since 2003. It will update daily from NASA's near-real-time fire data. We will also make a printable one-page calendar for each upazila, which we call *Agun Ponjika* (fire almanac), for notice boards in union offices and schools.

We will start with Mymensingh division, because it has both rice burning and brick kilns, and then expand to all of Bangladesh.

## NASA data

| Dataset | What we use it for |
|---|---|
| FIRMS MODIS active fire (Terra, Aqua) | Fire record from 2003 |
| FIRMS VIIRS 375 m active fire (S-NPP, NOAA-20, NOAA-21) | Fire record from 2012, plus the current season |
| MOD14A1, MYD14A1, VNP14A1, VJ114A1 daily fire masks | Cloud-free area each day |
| Black Marble VNP46A2 night lights | Checking whether kilns are active at night |
| ECOSTRESS and Landsat surface temperature | Checking whether sharper heat sensors can see kilns |

We also use Sentinel-1 and Sentinel-5P (ESA), ESA WorldCover, the APAD brick kiln map of Bangladesh, and district and upazila boundaries from HDX. Full details are in [docs/nasa-data.md](docs/nasa-data.md).

## How we will check our work

We are writing our pass/fail rules down now, before we build anything, so the results can't be adjusted afterwards. We will publish every result, including the ones that fail.

- Does the false 2012 jump go away after correction?
- Do our error ranges contain the true value about 95% of the time?
- During 2012–2021, does the corrected record match what Aqua MODIS saw? Aqua's sensor didn't change in those years.
- Do brick kilns show up in fire data more often than matched farmland?
- Will the method still work after S-NPP shuts down?
- Can people in Mymensingh find their area's burning season within a minute?

The exact rules are in [docs/test-plan.md](docs/test-plan.md).

## Who it helps

- Families and schools that want to know which weeks will be smoky
- Agriculture officers who plan awareness campaigns about straw burning
- Environment inspectors who plan when to visit kiln areas
- Disaster and early-warning teams that track unusual fire seasons
- Researchers and journalists who need a fire record they can trust

## Documents

- [Proposal](docs/proposal.md): the full concept, the website plan, and what we will not claim
- [NASA data plan](docs/nasa-data.md): each dataset, how we access it, and its licence
- [Test plan](docs/test-plan.md): our hypotheses and pass/fail rules
- [Hackathon plan](docs/hackathon-plan.md): roles, schedule and priorities for the two days
- [Event rules](docs/event-rules.md): key dates, the three required videos, and judging
- [Video scripts](pitch/videos.md)

## Status

- [x] Concept, data plan and test plan
- [ ] Video 1 and registration on the Bangladesh portal
- [ ] Team registered on spaceappschallenge.org (Mymensingh event)
- [ ] Read the full challenge statement (28 October) and update the plan
- [ ] Build during the hackathon (13–14 November)
- [ ] Videos 2 and 3, and the NASA project page

## Team

Team name and members will be added here.

## License

MIT. All datasets keep their original licences and are credited in [docs/nasa-data.md](docs/nasa-data.md).

Kiln Watch is not affiliated with or endorsed by NASA.
