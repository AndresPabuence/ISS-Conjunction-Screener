# ISS Conjunction Screening and Historical GP Sensitivity

I built this project to explore whether a student-built SGP4 screening pipeline could recover close approaches to the International Space Station from public CelesTrak GP data.

I also wanted to see how much the predicted time of closest approach and miss distance can change when different historical GP sets are used for the same encounter.

## Research question

**How well can a student-built SGP4 screener recover close approaches to the ISS from a frozen CelesTrak GP snapshot, and how much can the predicted TCA and miss distance change when the GP set changes?**

This is a research and learning project, not an operational collision-avoidance system.

---

## Workflow

The analysis:

- starts from a frozen CelesTrak active-satellite snapshot,
- reduces the catalog with a radial pre-filter,
- propagates candidate objects with SGP4 over a 72-hour window,
- detects separate close-approach intervals for the same object,
- refines each candidate event around its local minimum,
- compares the closest recovered events with SOCRATES values recorded during the experiment,
- and reconstructs one ISS–POLYTECH encounter using historical GP sets.

The full catalog screening used a 20-second coarse time grid.

The final screening threshold was 50 km. To avoid missing close approaches occurring between coarse samples, I used a 270 km coarse gate before numerical refinement.

---

## Screening results

The frozen active-satellite snapshot contained **16,636 objects**.

After radial pre-filtering, removal of ISS-related objects, and removal of duplicate NORAD IDs, **7,018 objects** entered the full screening.

| Metric | Result |
|---|---:|
| Active-satellite snapshot objects | 16,636 |
| Objects entering full screen | 7,018 |
| Coarse candidate intervals | 11,645 |
| Refined candidate intervals | 11,642 |
| Events below 50 km | 878 |
| Unique objects below 50 km | 816 |
| Events below 5 km | 3 |

Three coarse intervals touched the boundaries of the 72-hour search window. They were checked separately rather than passed through the normal local-refinement procedure. None produced an additional event below 50 km.

---

## Closest recovered events

| Rank | Object | NORAD ID | Minimum separation |
|---|---|---:|---:|
| 1 | SHERPA-LTC2 | 53754 | 1.989251 km |
| 2 | 2024-199AU | 61777 | 2.693989 km |
| 3 | POLYTECH UNIVERSE-4 | 61747 | 3.942206 km |

These were the only three recovered events below 5 km in the frozen screening dataset.

---

## SOCRATES comparison

The three closest recovered events were compared with SOCRATES values recorded during the experiment.

For the ISS–POLYTECH case, my calculation gave:

- **TCA:** 2026-10-05 10:36:27.477572 UTC
- **Minimum separation:** 3.942206 km
- **Relative speed:** 7.384565 km/s

The recorded SOCRATES values were approximately:

- **Minimum separation:** 3.942 km
- **Relative speed:** 7.385 km/s

The SHERPA-LTC2 and 2024-199AU encounters also reproduced their recorded SOCRATES miss-distance and relative-speed values within the precision shown by SOCRATES.

Because the SOCRATES values are rounded, the small numerical differences should not be interpreted as evidence of sub-meter physical accuracy.

---

## Historical GP sensitivity

I reconstructed the same ISS–POLYTECH encounter using historical GP sets collected before the conjunction.

For this event, the predicted time of closest approach remained comparatively stable while the predicted miss distance changed substantially.

Across the historical GP sets:

- predicted miss distance ranged from approximately **0.35 km to 14.84 km**
- maximum absolute TCA deviation from the validated reference was approximately **1.62 seconds**

For this single encounter, timing was much more stable than miss distance.

I would not generalize this result to all conjunctions, but it shows why a close-approach prediction should always be associated with the specific GP set that produced it.

---

## Notebook

The complete analysis is contained in:

[`ISS_Conjunction_Screener.ipynb`](ISS_Conjunction_Screener.ipynb)

The notebook contains the full propagation, screening, refinement, validation, historical-reconstruction, and visualization workflow.

---

## Python libraries

- NumPy
- Pandas
- Matplotlib
- SciPy
- sgp4

Orbital data were obtained from CelesTrak.

---

## Limitations

The source catalog is the saved CelesTrak active-satellite snapshot used in this experiment, not the complete debris catalog.

The radial pre-filter is based on GP mean elements. It is a practical computational reduction step, not a proof that every possible conjunction in an arbitrary catalog would be retained.

The 72-hour screening uses a frozen GP snapshot and does not model later orbital updates or maneuvers.

The analysis calculates TCA, miss distance, and relative speed. It does not include state covariance information and therefore does not estimate probability of collision. It is not a substitute for operational conjunction data messages (CDMs).

The ISS-module and docked-vehicle exclusion list is specific to the saved snapshot.

In the historical study, GP element epochs are used as update markers. They should not be interpreted as exact publication or operational availability times.

SOCRATES is dynamic, so values recorded during this experiment may differ from values shown later after newer orbital data are ingested.

---

## Author

**Andres Felipe Pabuence Badillo**

October 2026
