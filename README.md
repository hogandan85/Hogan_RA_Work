 # Impact Fund Screening — Research Assistant Work for Professor Dirk Jenter

Text-extraction and classification pipelines supporting ongoing research (Dasgupta, Jenter et al.) into the prevalence and strategies of "impact" investing among European mutual funds — firms taking equity stakes to influence corporate practices while marketing themselves as green-focused to investors.

The underlying dataset covers 5,000+ funds' regulatory disclosures (prospectuses, KIIDs, PRIIPS KIDs, strategy descriptions) across multiple languages and formats. The project has two components, run independently.

## 1. Fund Name Screener — `Impact_Fund_Screener.ipynb`

Identifies funds whose **name** suggests an impact or sustainability mandate, using regex matching against an English-plus-multilingual keyword list (French, German, Italian, Spanish, Nordic languages, and others).

- Token-level abbreviation expansion, so ambiguous short forms (e.g. "Sust", "Trans") are resolved rather than silently matched or missed
- A `needs_review` flag on ambiguous or high-false-positive-risk terms, routed to manual inspection rather than auto-classified
- Output to Excel across four sheets: per-match hits, deduplicated funds, pattern-level summary statistics, and unknown-abbreviation candidates

**Run:** edit Cell 1 (paths/columns) and Cell 2 (keyword patterns) if needed, then Kernel → Restart & Run All.

## 2. Full Extraction Pipeline — `Sample_Testing/Pipeline_Full.ipynb`

Classifies each fund's **stated objectives** (from disclosure text, not just its name) as financial or sustainability-focused, via a three-pass LLM pipeline using the Claude API.

- **Pass 1 — Extract:** pulls stated objectives independently from each disclosure column (prospectus, KIID, PRIIPS KID, strategy description, and translations), separating *what* the fund aims to achieve from *how* it pursues it.
- **Pass 2 — Consolidate:** merges the per-column extractions into a single objective list per fund, resolving duplicates and cross-language equivalents.
- **Pass 3 — Verify:** checks each consolidated objective against the original source text and assigns a confidence rating (high / medium / low / none), flagging anything it can't verify.

An earlier version split extraction across multiple LLM calls; benchmarking against a single-call, few-shot-prompted approach found the latter more reliable, and the pipeline was rebuilt accordingly.

**Known issue and fix:** smart quotation marks in source text broke JSON parsing of the model's output. A combination of pre-parsing normalization and an explicit system-prompt instruction resolved the large majority of parse failures encountered in testing.

## Repository structure

```
Impact_Fund_Screener.ipynb          Current fund-name screener (see §1)
Sample_Testing/
  Pipeline_Full.ipynb                Current three-pass LLM extraction pipeline (see §2)
  Impact_Fund_Screener.ipynb         Earlier development copy — smaller keyword set
Fund Goal Extraction (Failed)/       Deprecated first-generation pipeline (see below)
README.md
```

**On `Fund Goal Extraction (Failed)/`:** this holds the original implementation of the objective-extraction pipeline — separate `Pass_1`/`Pass_2`/`Pass_3` notebooks, a full-dataset batch-processing run via the Claude Batch API, and a review tool for triaging low-confidence results. Benchmarking this multi-call approach against a single-call, few-shot-prompted design (now in `Sample_Testing/Pipeline_Full.ipynb`) found the latter more reliable, and the pipeline was rebuilt accordingly. This folder is retained as a record of that comparison, not as an abandoned or broken approach.

## Outputs

Latest batch outputs: [Google Drive link]

## Models used

- Prior to May 17, 2026: Claude Sonnet 4.5
- May 17, 2026–present: Claude Sonnet 4.6
