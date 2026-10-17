# Preliminary retrieval baseline

Run date: 18 October 2026. This is the **lexical TF-IDF baseline**, not the optional sentence-embedding experiment.

## Setup

- Query representation: public comment text recorded on or before the ticket's first assignment.
- Candidate corpus: earlier text-bearing tickets whose final-resolution timestamp was observed before the query timestamp.
- Similarity: TF-IDF unigram/bigram cosine similarity.
- Agent ranking: sum nonnegative similarity votes from the five most similar eligible historical tickets.
- Evaluation: chronological split; only text-bearing tickets are included.
- Private comments excluded.

## Results

| Metric | TF-IDF retrieval/ranking | Past majority-resolver baseline |
|---|---:|---:|
| Top-1 hit rate | 22.1% | 10.3% |
| Top-3 hit rate | 27.9% | Not applicable |
| Top-5 hit rate | 29.4% | Not applicable |

The chronological split included 339 tickets with eligible public text (271 before the split; 68 test tickets with a sufficient historical corpus). Top-1 retrieval accuracy exceeded the majority resolver on this small test subset.

## Interpretation and limitations

- Only 339 of 35,665 ticket-history IDs have public text as of the first assignment (~0.95% coverage).
- Median public comment length is 12 characters.
- This measures retrieval of the **historical final resolver**, not the best agent, outcome quality, SLA compliance, or causal policy improvement.
- It is not evidence of performance on the remaining ticket population.
- SentenceTransformer embedding mode is configured as an optional comparison but was not run in this initial audit.
- Next: run the embedding comparator, retain identical time/split/eligibility logic, and compare Top-k results and coverage. Then proceed to the separate supervised baseline with outcome-observable chronological folds.
