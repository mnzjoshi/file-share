# Hand anatomical rubric, version 2.1-draft (H0 calibration, production keyframes)

**Status:** **FROZEN DRAFT for this H0 round.** Both raters use exactly this text. The file's SHA-256 is recorded in `ARTIFACT_HASHES.sha256` and in each package manifest.
- It becomes a versioned `v2.x` only after H0 is complete and reviewed; see `H1_ENTRY_CRITERIA.md`.
- If H0 changes it materially, a new independent calibration round is required.

**Change log:**
- **v2.0-draft (2026-11-13):** first H0 draft, based on repaint items shown beside a reference.
- **v2.1-draft (2026-11-14):** adapted for unmodified production keyframes:
  - there is no reference image, and G is N/A;
  - criterion A records five fields;
  - criterion B records presence, position, orientation and attachment separately;
  - a reason is required for every Borderline or Fail;
  - a natural-occlusion guide is added.
  - The criteria and outcome rules are otherwise unchanged.

## Who may rate

- **Only humans.**
  - Rater 1: the project owner.
  - Rater 2: an independent person with visual-assessment experience.
- AI systems (Claude, ChatGPT, others) are **never** raters and never approve. AI output is advisory at most.

## How to rate one item

1. Open `<ID>_frame.png` (the whole picture, with the hand region outlined in yellow) and `<ID>_hand.png` (the same region enlarged). `ITEM_LIST.md` says which hand to rate.
2. Write down the **view** first: back of hand, palm, thumb side, little-finger side, fist front (knuckles toward the viewer), open, pointing, or holding an object.
3. Fill in criteria A–H, then the overall classification. For Borderline or Fail, write a short reason.

## Criteria

**A. Digit completeness (essential).** Record all of these fields:
- **expected digits** for this view;
- **visible or reasonably inferable digit structures**;
- **naturally occluded digits**;
- **missing or duplicated digits**;
- **uncertain anatomy**.

| Result | When |
|---|---|
| Pass | the visible digits plus the naturally occluded ones account for a thumb and four fingers |
| Fail | a digit is missing with nothing hiding it, a digit is duplicated, or digits are fused |
| Unsure | occlusion or drawing style prevents a confident count |

**Five separately visible fingertips are never required for a clenched fist or a gripping hand.**

**B. Thumb anatomy (essential).** Record presence, position (the correct side for this hand and view), orientation (nail direction) and attachment.

| Result | When |
|---|---|
| Pass | present, or hidden by plausible occlusion; on the correct side; plausible length, joint and attachment |
| Fail | absent without explanation; on the wrong side for the hand and view; implausible length, joint or attachment |
| Unsure | cannot tell |

**C. Finger separation (essential).** Are the visible finger structures distinct and anatomically plausible?
- **Pass:** the separations are consistent with the view.
- **Fail:** the digits merge into a mass where the view should show them separately.

**D. Hand contour (essential).** The palm, wrist transition, fist silhouette and overall structure.
- **Pass:** a plausible hand shape that joins the wrist.
- **Fail:** a broken wrist line, an impossible bulge, or a blob.

**E. Face-like features.** Classify as **None / Possible / Clear**. This is about faces that appear *within the hand itself*. A real face that the hand touches, or a printed face on an object, is not a hand artifact.
- **Clear:** an automatic Fail.
- **Possible:** the item can be Borderline at most.

**F. Overall anatomical plausibility (essential).** Does it read as a natural human hand, rather than only an attractive shape? Pass / Fail / Unsure.

**G. Preservation and compositing.** **N/A for every item in this round**, because all items are unmodified originals.

**H. Surface artifacts.** Recorded separately as **None / Minor / Material**, with a description: creases, unusual shading, texture, stray marks, heavy outlines.
- **Minor surface marks never fail an otherwise correct hand on their own.**
- Use **Material** only when a mark changes how the anatomy reads, for example a crease that looks like a digit boundary. Record it under A–F as well.

## Natural occlusion guide

| View | Typically visible | Typically hidden (not missing) |
|---|---|---|
| Fist, back of hand | 4 knuckles or finger rolls; the edge of the thumb at one side | fingertips; much of the thumb |
| Fist, thumb side | thumb (tip or nail) over the curled index finger; 3–4 stacked finger rolls | fingertips; the little finger partly |
| Fist, front (knuckles toward the viewer) | 4 finger segments in a row; thumb across or beside them | the palm |
| Open hand, back or palm | 5 digits with separations | none, or by overlap |
| Pointing | an extended index finger; the other fingers curled | the curled fingertips; often the thumb |
| Holding an object | wrapped digits, usually with the thumb opposite the fingers | the digits behind the object |
| Hand truncated by the picture edge | what is inside the frame | what is outside the frame. Treat this as occlusion, not as missing |

**Rule:** a digit counts as *naturally occluded* only if something plausible hides it: the hand's own pose, an object, another body part, or the picture edge. Otherwise it is **missing**.

## Overall classification

| Outcome | When |
|---|---|
| **ACCEPTABLE** | the essential criteria A, B, C, D and F pass; E = None; G = N/A; H at most Minor. No material digit or thumb defect, no clear face-like interpretation, no unacceptable deformation |
| **BORDERLINE** | no Fail, but anatomy is uncertain (any Unsure on A, B or F); occlusion or ambiguity prevents a confident judgement; or E = Possible. **Human adjudication is required** |
| **FAIL** | missing or duplicated anatomy; a clearly implausible digit structure; E = Clear; material deformation (C or D fails) |

**Hard rules:**
- **Borderline and Fail are never published automatically.**
- There is **no automatic acceptance**.
- **Any disagreement between the raters goes to human adjudication.**
- The original image is kept until a decision is made.

## Combining two raters

| Rater 1 | Rater 2 | Combined |
|---|---|---|
| Acceptable | Acceptable | Acceptable |
| Fail | Fail | Fail |
| Borderline | Borderline | Borderline (a human decides on use) |
| any differing pair | | **adjudication**: a third human, or a joint review with reasons recorded |

## Points to resolve in H0 (all open)

1. Should E = Possible be Borderline (as drafted) or Fail?
2. For the thumb-side fist, is "thumb + 3 rolls" a Fail on A, or Unsure?
3. When is a crease Material under H, rather than a digit boundary under A or C?
4. **New in 2.1:** how should gloved hands and multi-person overlaps be scored? Count digits through the glove? Score a hand only when its digits can be attributed to one person?
