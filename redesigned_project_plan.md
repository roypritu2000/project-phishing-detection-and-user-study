# Redesigned project plan

## Working title

**When Confidence Misleads: Communicating Unstable Phishing-Detector Scores**

## Main question

> When a phishing detector may be wrong, does an honest warning about the weakness of its current score help users make better decisions?

## Why this is different

Existing PhiUSIIL papers already compare models, select features, test outside datasets, and alter URLs to see whether models fail. Existing human studies already compare warning designs and describe a detector's overall reliability.

This project connects the model's behavior on each selected example to the message shown to the user. It focuses especially on wrong, overconfident, or unstable answers rather than reporting another near-perfect average accuracy.

## Scope

- Keep PhiUSIIL as the main dataset.
- Use supervised learning only.
- Use URL text only; never open or request a listed website.
- Use one simple model and one tree-based model during development.
- Treat probability adjustment as preparation, not the main discovery.
- Do not claim real-world deployment readiness.
- Use safe local mock pages for the user study.
- Complete within approximately six weeks.

## Research questions

### Model questions

1. How much do PhiUSIIL's easy shortcuts inflate model scores?
2. After those shortcuts are removed, which test examples receive confident but incorrect answers?
3. Which answers remain stable, and which change substantially after harmless changes to the written URL?

### User question

4. When the model is wrong or unstable, does disclosing that weakness help users reject its advice while preserving useful reliance when it is correct?

## Phase 1: Confirm the gap

Create a short literature table covering:

- PhiUSIIL model comparisons
- PhiUSIIL feature selection
- Testing on separate datasets
- Tests using altered URLs
- Probability quality
- Phishing-warning reliability and trust
- Warning explanations

Before recruiting participants, repeat the exact-title and keyword search in the university's Google Scholar, Scopus, or Web of Science access. Stop or revise if a study already tests case-specific score instability in phishing warnings.

Deliverables:

- `existing_work_review.md`
- `related_work.csv`
- Final one-paragraph novelty statement

## Phase 2: Prepare the dataset safely

1. Rename the target so `1` clearly means phishing.
2. Remove duplicate URLs.
3. Keep related domains in the same data portion.
4. Create training, score-adjustment, and final test portions.
5. Do not use `FILENAME`, titles, raw domain identifiers, webpage-content measurements, or precomputed similarity/probability fields in the main model.
6. Build a small, documented URL parser that calculates simple measurements locally.

Main measurements may include URL length, number of subdomains, digits, separators, unusual characters, and path/query lengths. HTTPS should be tested separately because every legitimate PhiUSIIL row uses it.

Deliverables:

- Clean split files containing row identifiers only
- URL parser with tests
- Updated dataset report

## Phase 3: Train only necessary models

Train:

1. Logistic regression
2. Random forest

Use XGBoost only if time remains; comparing many models is not the research contribution.

For transparency, report three results:

- All numeric fields, clearly labeled as an inflated diagnostic result
- Safe URL-only fields
- Safe URL-only fields without HTTPS

Choose one final model using validation data. Keep the final test data untouched until the design is fixed.

## Phase 4: Make the scores interpretable

Use a separate portion of the data to adjust the model's scores. Compare a simple S-shaped adjustment and an ordered flexible adjustment. Report:

- Classification performance
- Average squared probability error
- A graph comparing predicted scores with observed results
- The number of examples in each score range

Call the output a **model phishing score**, not a real-world risk percentage.

## Phase 5: Test whether individual answers are stable

Apply harmless text-only changes without opening the URLs. Examples include:

- Changing capitalization in the scheme or host
- Adding a fragment such as `#section`, which is not sent to the website
- Using equivalent writing for safe characters in the path

For every final-test URL, record:

- Original model score
- Lowest and highest score across equivalent versions
- Whether the predicted class changes
- Whether the original prediction is correct

This phase produces the information needed for honest warning messages. It does not attempt to invent new phishing attacks.

## Phase 6: Select study cases

Select approximately 12 held-out cases, balanced across:

- Correct and stable
- Correct but unstable
- Incorrect and stable
- Incorrect and unstable
- Phishing and legitimate labels

Do not choose only obvious or extreme cases. Manually verify every selected URL and model result as inert text. Build safe local pages; do not copy live page content or collect credentials.

## Phase 7: Warning experiment

Use two warning styles so the study remains manageable.

### Condition A: score only

> Potential phishing website  
> Model phishing score: 81%  
> We recommend leaving this page.

### Condition B: score plus honest limitation

> Potential phishing website  
> Model phishing score: 81%  
> This result is unstable: harmless changes to the written URL changed the model's assessment.  
> Check the domain carefully or leave this page.

For stable cases, the second warning should say that the result remained stable under the same checks. Do not imply that “stable” means correct.

Each participant sees both warning styles, with cases and order balanced across participants. For each case, collect:

- Continue or leave
- Confidence in that decision
- Whether the participant followed the model
- Optional short reason

Primary outcome:

> Appropriate reliance: following correct model advice and rejecting incorrect model advice.

The most important comparison is behavior on incorrect or unstable cases. Overall browsing accuracy is secondary.

## Phase 8: Analysis

For each participant, calculate:

- Decision accuracy under each warning
- Appropriate reliance under each warning
- Following incorrect advice
- Rejecting correct advice
- Average decision confidence

Use paired comparisons because every participant sees both conditions. Report the size of the difference and its uncertainty, not only whether a statistical test crosses a cutoff.

Treat a sample of 15–30 people as an exploratory course study. Do not claim broad population effects from it.

## Go/no-go rules

Proceed only if:

- The safe URL-only model produces enough correct and incorrect held-out cases.
- The stability check produces a meaningful mix of stable and unstable results.
- The warning text passes a 2–3 person pilot.
- Course and human-study approval is confirmed before recruitment.

If the model produces too few errors, do not manufacture them. Use naturally occurring errors from a separately held-out static dataset or change the project to an ML-only audit of misleading confidence.

## Six-week schedule

| Week | Work |
|---|---|
| 1 | Final literature check, exact research statement, safe URL parser |
| 2 | Cleaning, grouped splits, two baseline models |
| 3 | Score adjustment, shortcut tests, stability checks |
| 4 | Case selection, mock pages, warning prototype |
| 5 | Pilot, revisions, approved participant sessions |
| 6 | Analysis, limitations, final report |

## Minimum final contribution

The project succeeds if it can answer:

> Does showing an honest, case-specific limitation help people avoid blindly following a phishing detector when its confident answer is wrong?

That is more distinct and useful than another model leaderboard on PhiUSIIL.
