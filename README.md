# RLVR Instrumental Convergence

Testing whether RLVR (Reinforcement Learning from Verifiable Rewards) training induces instrumental convergence behaviors in LLMs, using AllenAI's Olmo-3-7B-Think training checkpoints and the InstrumentalEval benchmark.

## What this is

A proof-of-concept pipeline that:
- Loads Olmo-3-7B-Think at different training checkpoints (4-bit quantized, runs on free-tier Colab GPU)
- Generates responses to real InstrumentalEval scenarios (deception, shutdown evasion, hacking, hiding behavior, self-replication, strategic alignment-faking)
- Scores each response for instrumental convergence using two independent LLM judges (Gemini)
- Computes inter-judge agreement rate (IAR) as a check on judge reliability

## Status

Early-stage, actively in progress. Zero-budget: free-tier Colab GPU + free-tier Gemini API as judges.

**Known issue, now fixed:** initial checkpoint selection sorted branch names alphabetically rather than numerically by training step, which could pick the wrong checkpoints for "earliest vs. latest" comparison. Fixed by parsing and sorting on the numeric step count.

**Initial finding:** first full run (21 of 24 cases judged, 3 hit free-tier rate limits) showed 85.71% inter-judge agreement — meaning roughly 1 in 7 cases had judges disagreeing on whether a response showed instrumental convergence. This is a live open question for the project: how reliable are LLM-judge evals at catching subtle/indirect misalignment signals?

## Data source

Prompts are pulled programmatically from the original [InstrumentalEval benchmark](https://github.com/yf-he/InstrumentalEval) (introduced in "Evaluating the Paperclip Maximizer," He et al.), not hand-written.

## Next steps

- Re-run full dual-judge pass across all 6 categories with corrected checkpoint sorting
- Expand from earliest/latest checkpoint pair to a fuller checkpoint sweep
- Investigate the judge-disagreement cases directly to understand what's driving the gap

## Setup

Requires a Gemini API key stored in Colab Secrets as `API`. See `rlvr_instrumental_convergence.ipynb` for the full pipeline.
