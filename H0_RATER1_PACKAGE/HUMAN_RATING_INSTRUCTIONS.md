# H0 calibration: instructions for human raters

You are one of **two human raters**. Your scores calibrate a hand-anatomy rubric. They are not a test of you, and there are no "right answers" in this package.

## Before you start

- Work **alone**:
  - do not discuss the items with the other rater until both of you have submitted;
  - do **not** use ChatGPT, Claude or any other AI tool to help.
- Open **only** your own package folder.
- If anyone shows you earlier ratings or tells you where the images came from, stop and tell the coordinator.
- Allow about **30–40 minutes**. You can take breaks.

## What is in your package

| File | Use |
|---|---|
| `HAND_ANATOMICAL_RUBRIC_V2.md` | the scoring rules. **Read them first** |
| `NATURAL_OCCLUSION_GUIDANCE.md` | which digits may be hidden for each pose |
| `ITEM_LIST.md` | the items, in your order, and which hand to rate in each |
| `images/<ID>_frame.png` | the whole picture; the region to rate is outlined in yellow |
| `images/<ID>_hand.png` | the same region enlarged. Zoom in further if you need to |
| `RATER?_CALIBRATION.csv` | your score form, one row per item |
| `SUBMISSION_AND_HASH.md` | how to return your form |
| `PACKAGE_MANIFEST.sha256` | checks that your package is complete and unchanged |

## Scoring

1. For each item, record the **view** first. Then fill in **every column** (A–H, then OVERALL).
2. **Count digits carefully:**
   - write what you expect for the view, what you see, what is naturally hidden, and anything missing or duplicated;
   - for example: "expected 5; see thumb + 4 rolls; little fingertip hidden by the fist".
3. Use **Unsure** whenever you genuinely cannot decide. Uncertainty is useful information.
4. **G (preservation)** is already filled in as **N/A**: every picture is an unmodified original.
5. For every **BORDERLINE** or **FAIL**, write a short reason in `reason_if_borderline_or_fail`.
6. Fill in the first lines of the form: your **name**, the **date**, and the **declaration** (yes/no).

## Important

- Rate **what you see**. Do not guess how the picture was made.
- Some items may look clearly fine and some clearly wrong. Rate them all the same way.
- Once submitted, your file is **frozen by its hash**. Corrections can only be added afterwards as separate notes, never by editing the frozen file.
