# Research mode and evaluation

## Study panel
Open **Study** in the top bar.
- **Ablation switches**: Memory, Emotion, Preference learning, Reputation.
- **Ladder presets**: C0 static → C1 +memory → C2 +emotion → C3 full → C4 +reputation.
- **Live metrics**: dialogue turns logged, preference convergence, emotional consistency, memory-reference rate.
- **Data**: enter a participant ID, then submit the session to a collection endpoint (for example a Google Apps Script web app writing to a sheet) or export it as **JSON** or **CSV**.
- **Demo triggers**: world events (raid, merchant, festival, stranger), bosses and weather.

Logged per dialogue turn: timestamp, turn index, character, condition, register offered and chosen, whether the prediction matched, number of retrieved memories, whether the reply overlapped a retrieved memory, the four affect values, reply length, a sentiment score and a consistency flag.

## EMBER-PROBE benchmark
- 12 character–player stores × 60 events = **720 events**, generated from the game's event grammar with one seed.
- Each event gives one memory and two probes: a **lexical** probe (reuses the memory's words) and a **paraphrase** probe (same event, different words): **1,440 probes**.
- Ground truth is fixed by construction: exactly one correct memory per probe; the other 59 records are realistic distractors.
- Eleven retrieval policies compared: random, recency, importance, BM25, TF-IDF, LSA, Generative-Agents score, MemoryBank retention, Emberfall lexical, Emberfall embedding and overlap-only.
- Metrics: Hit@1, Hit@4, MRR, with bootstrap 95% confidence intervals, paired permutation and McNemar tests, Holm correction.

## Key findings
| Finding | Evidence | Repair identified |
|---|---|---|
| Retrieval collapses when the player paraphrases | Hit@4 falls from 0.994 to 0.068; a pretrained encoder recovers 0.528 | Make the embedding path the default and drop the fixed importance term |
| Capacity, not ranking, is the binding constraint | About half of all events are evicted at capacity 30 | Raise capacity; summarise instead of deleting |
| Affect defaults saturate | Trust reaches its maximum within three turns | Lower the per-turn bound from 25 to 4 |
| The register update adapts slowly | 13.8 turns to recover after a preference change vs 2.2 for DPO | Keep per-player learning, replace the update rule |
| Two logged proxies do not measure what they name | Memory proxy fires on 28% of memory-free replies | Human annotation and direct measurement |

## Limitations
No human-subject study has been run yet, so nothing here shows whether players find the characters more believable. The benchmark uses a templated grammar, and the register experiments use a simulated player.
