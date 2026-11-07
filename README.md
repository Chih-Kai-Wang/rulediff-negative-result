# RuleDiff Negative-Result Technical Report

This repository publishes the technical report *When 0.99 Becomes 0.67: A Confirmatory Failure of Lexical Policy-Impact Prediction* by Chih-Kai Wang, National Taipei University of Education, with the frozen artifact that recomputes its numbers.

The report asks whether a cheap lexical model can predict which compiled policy guards go stale when a natural-language policy changes. On the development packet, the frozen lexical model scored 0.9919 stratified macro-F1. We then evaluated it once on a separately built retail packet, where it scored 0.6661 and missed 14 affected guards. We kept that result as the terminal answer and did not retune on it.

The report has not been peer reviewed, accepted, or published in MLSys proceedings. It does not claim that any unsafe execution was observed, and it does not offer a validated end-to-end safety guarantee.

## Contents

- `main.pdf`: four-page public technical report.
- `main.tex` and local style files: report source.
- `references.bib`: bibliography.
- `artifact_bundle.tar.gz`: frozen reproducibility package.
- `CITATION.cff`: citation metadata.
- `LICENSE`: MIT license for author-created material.
- `PUBLICATION_MANIFEST.sha256`: release checksums.
- `tools/pack_artifact_bundle.py`: deterministic packer that rebuilds `artifact_bundle.tar.gz`.

## Reproduce the evidence

Extract the artifact archive, enter its `artifact` directory, and run:

```bash
python3 verify_artifact.py
```

The last line printed should be `ARTIFACT_VERIFICATION_PASS`. The verifier needs Python 3.11 or newer, with no third-party packages and no network access. It recomputes every reported development aggregate and every terminal private aggregate.

## Revisions

- v1.0.1 reissued `artifact_bundle.tar.gz` so that the verifier passes on Python 3.12 and newer and after extraction with GNU tar. Python 3.12 switched `sum()` over floats to compensated summation. That moves the recomputed G3 stratified macro-F1 and the upper bootstrap endpoint by one unit in the last place, about `1.1e-16`. The verifier now compares these two values with the `1e-12` absolute tolerance it already applied to development scores. The v1.0.0 bundle (sha256 `3291bb5d…109a1`) also contained macOS `._*` metadata entries, which broke the manifest check on Linux. Data, labels, evaluator code, and recorded results are byte-identical between the two versions. Only `verify_artifact.py`, the bundle `README.md`, and `MANIFEST.sha256` changed.

## Scope

A full-paper follow-up aimed at a positive claim was stopped under its preregistered rule. Even oracle selective routing was slower than guard-only lazy recompilation in all four dense primary regimes. This release is the frozen negative-result fallback. It does not revive the rejected full-paper claim.
