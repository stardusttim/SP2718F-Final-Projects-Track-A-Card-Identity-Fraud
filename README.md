# SP2718F-Final-Projects-Track-A-Card-Identity-Fraud

Google File: https://docs.google.com/document/d/1TdUgnc7p4SwYVnWcoGZYd3GV5Ad5bGyEcMlWE00RV8w/edit?tab=t.0

Hackathon Sheet: https://docs.google.com/spreadsheets/d/1c3wA9ShoJ5GGNudVLQKrfFPqn-bV34FsHRJr6zTWbbw/edit?gid=0#gid=0

Chatgpt Group: https://chatgpt.com/g/g-p-6a505fd158a48191a6e3f0c9c2949177/project

# Track A — Card & Identity Fraud
### "Who is really making this purchase?"

*SP2718F Group Project — Days 10–15*

---

## How This Fits the Project

Every track in this project is investigating the same underlying problem: **card-not-present (CNP) fraud** in the IEEE-CIS dataset. What makes your track different from the other three isn't the dataset — it's the lens. Track A's stakeholder is the **issuer** — the cardholder's own bank, whose card-not-present risk team is asking a specific version of the fraud question: *does the evidence in this transaction actually match the person who's supposed to be making it?*

That stakeholder framing should shape your work from Stage 1 (Problem Framing) onward — your research questions, your success metric, and eventually your recommendation should all be written with the issuer in mind, not a generic "detect fraud" brief. Stages 2–4 (EDA, Clean & Prepare, Build) will look broadly similar across tracks; Stages 1, 5, and 6 are where your group's work should look distinctly like Track A's.

**A note on everything below:** the business framing, research questions, and analytical suggestions in this brief are starting points, not established facts about this dataset — treat them as hypotheses to test and adapt as you go, not instructions to follow.

---

## Business Framing

One plausible narrative for this kind of fraud: someone other than the cardholder has obtained the card details — through phishing, a data breach, or skimming — and knows the card number and expiry, but not the finer details a genuine cardholder would have (their own billing address, their usual email domain, and so on). If that narrative holds, it should show up as mismatches: billing address inconsistencies, unusual email domains, or match-flag failures. Whether it actually holds for this dataset, and to what degree, is for your group to establish.

**A question worth exploring:** does this transaction's supporting evidence actually match the cardholder it claims to belong to — and what would the issuer's risk team need to see to be convinced either way?

---

## Research Questions to Get You Started

1. Which combinations of M-flag failures (M1–M9) are most predictive of fraud?
2. Is the mismatch between purchaser email domain and recipient email domain a strong fraud signal?
3. How does fraud rate vary by card type (card4: Visa/MC/AmEx/Discover) and card category (card6: credit/debit)?
4. Can you build a model that performs well using *only* the card, address, and email features (no V-features)?

These are starting points, not a checklist — your group should treat them as hypotheses to test, refine, or replace once you've seen the data. Bring your own questions too.

---

## Primary Features

`card1–card6`, `addr1`, `addr2`, `dist1`, `dist2`, `P_emaildomain`, `R_emaildomain`, `M1–M9`

---

## Analytical Approach — Ideas Worth Testing

These are suggestions to try and validate against your own EDA, not a required sequence — if the data points you somewhere else, follow it.

- **EDA:** a heat map of M-flag combinations against fraud rate could be revealing; email domain frequency analysis is another angle worth a look
- **Feature engineering:** possible starting features include a `cards_match` composite flag, an `email_domain_type` grouping (commercial/anonymous/business), and frequency-encoded `card1` — test whether each one actually earns its place
- **Modelling:** logistic regression is often a reasonable first choice here for interpretability; a decision tree is worth trying too, for rule extraction
- **Required baseline:** build a simple rule-based baseline before any model — e.g. "flag if two or more M-flags fail" — and report every model's precision/recall against it. This isn't optional: it's what lets you say whether the added complexity of a model actually buys anything, and it's exactly the comparison an issuer's risk team would ask for.
- **Evaluation:** a precision-recall curve is a natural fit alongside the baseline comparison above
- **Leakage reminder:** any "high-risk" list or frequency encoding — email domains, `card1` groups, address values — must be computed from training data only, then looked up (not recomputed) for validation/test rows. This is an easy place to leak without noticing.

**A note on `card1`:** it's an anonymised/masked identifier, not a real card number or true BIN (bank identification number). Refer to groups of it as "high-risk `card1` groups" or "BIN-like identifiers" — not "card BIN ranges," which implies information this field doesn't actually carry.

---

## Pipeline Ideas — Assign One Per Team Member

Starting ideas for the team to divide up and adapt — not a fixed menu. Minimum 4 pipelines for a four-person team, 5 for the team of five. Mix types where you can — don't have everyone build the same kind of model.

1. **A candidate primary model** — logistic regression or decision tree on card/address/email features.
2. **A rule-based allow/block list** — consider block-listing email domains or high-risk `card1` groups with historically high fraud rates (computed from training data only, then looked up for validation/test), or building a hard rule from whichever M-flag failure combination turns out most predictive.
3. **A second ML model on a narrower slice** — e.g. M-flags only, or email/address only — to see which sub-signal carries more of the fraud signal, and where the two models disagree.
4. **An unsupervised novelty check** — e.g. k-Means on card/address/email features (no label), flagging transactions whose combination looks unlike any training cluster.
5. **(Five-person team)** A per-card-scheme model — e.g. separate lightweight models per `card4` value — testing whether fraud patterns differ enough by card scheme to be worth splitting.

**Combination:** one option is to combine the team's pipelines with a decision layer — e.g. "block if the rule-based pipeline fires OR any model score exceeds its threshold" — extended to however many pipelines your team ends up building. Validate that this combination logic actually improves on the pipelines individually before presenting it as your answer.

---

## Deliverable Emphasis

One way to package this: a **"fraud rule book"** — human-readable rules extracted from your models, with precision and recall reported for each — alongside your block-list pipeline and the combined decision layer. Treat this as a suggested shape for the deliverable, not the only valid one; adapt it if your group's findings point somewhere more compelling.

---

## Remember

- **Iterate.** Build something, evaluate it honestly, and improve it — demonstrated effort to improve over the week is part of what's assessed, not just your final number.
- **External resources are encouraged** — blog posts, Kaggle notebooks, papers on this dataset — but acknowledge the source and validate it with an instructor or TA before relying on it in your pipeline.
- **Three deliverables, three audiences:** a Demo (technical/hands-on), a Presentation (broader room, plain language), and a written Report (the detailed record). Whoever built a given pipeline leads the part of the demo/presentation that covers it.
- Book domain questions (is this framing realistic? does this finding make business sense to an issuer?) with the course instructor; book technical questions (modelling, features, leakage, code) with the additional instructor or the TA.
