# Lung cancer · radiotherapy evidence dashboard

Static GitHub Pages site (vanilla JS, no build step) summarising survival, local control, toxicity and
dose constraints from landmark lung cancer radiotherapy trials. Companion to the
[endometrial adjuvant dashboard](https://roresource.github.io/endometrial-evidence-dashboard/).

Live: https://roresource.github.io/lung-rt-evidence-dashboard/

## Structure

- `index.html` – UI (group selector + tabs: Overview, Survival, Local control, Toxicity, Constraints, Trials).
- `data.js` – all data: `GROUPS`, `TRIALS`, `OUT` (arm-level survival/control rows), `LC` (within-trial
  randomised local-control comparisons), `TOX` (toxicity rows), `CONSTRAINTS` (dose–volume predictors), `BOTTOM` (key messages).

Groups: `es` NSCLC stage I–II · `la` NSCLC stage III (incl. PORT) · `om` NSCLC oligometastatic/metastatic · `ls` SCLC limited · `ex` SCLC extensive.

Treatment types (`tx`): `rt` conventional RT alone · `hypo` hypofractionated/accelerated · `sabr` · `sabrio` SABR + IO ·
`scrt` sequential CRT · `ccrt` concurrent CRT · `crtio` CRT + IO · `port` · `pci` · `pall` palliative thoracic RT alone · comparators `surg`, `sys`, `none`.

`rt`, `hypo`, `sabr` and `pall` are single-modality RT (no chemotherapy) and are grouped separately on the Overview. Hypofractionated
chemo-RT (e.g. SOCCAR 55 Gy/20 + cisplatin–vinorelbine) is coded `ccrt`/`scrt`, not `hypo`. Rows flagged `sub:true` (post hoc
subgroups) appear on the Survival tab but are excluded from Overview ranges.

`ILD` (in `data.js`) feeds the ILD tab: relative-risk rows, modality-specific rates and practical points for patients with
interstitial lung disease / interstitial lung abnormalities. Sources are one phase II trial, systematic reviews and retrospective series.

## Conventions (same as the endometrial dashboard)

- Only author-reported numbers, transcribed from the full text (pdftotext) of the PDF library. Where the full text was not
  available the trial is tagged `src:"abs"` (published abstract) or `src:"rev"` (secondary review) and shown with a tag on the site.
- Nothing is pooled. "Typical range" on the Overview = min–max of arm-level point estimates for that treatment type within the group.
- Toxicity rows state grade threshold, acute/late and grading system; grades are never merged. Derived values (e.g. 100 − LC) are labelled.
- DOIs were verified against Crossref; where none was found `doi:""` and a PubMed search term (`pm`) is used.
- No PDFs or spreadsheets are committed.

## Change log

- v1 (2026-10-06): initial build from the full-text library.
- v3 (2026-10-07): 33 further full texts – abstract/review tags removed for RTOG 0236 (incl. 5-yr), RTOG 0813, Timmerman 2006, LUSTRE,
  Aupérin 2010, RTOG 9410, RTOG 0617 (5-yr), PACIFIC (2017/2018/5-yr), CHART (1997/1999), PORT meta-analysis, PORT-C, Turrisi,
  ADRIATIC, Aupérin PCI 1999, Slotman 2007, IMpower133; Grønberg final survival. New: PACIFIC-2, PACIFIC EGFRm, Zatloukal, RTOG 9311,
  NCCTG 89-20-52, Sibley, Qiao, MRC F2 vs F13, Gregor PCI, Graham V20, Kwa MLD, Belderbos. New ILD tab. Single- vs combined-modality
  split on Overview. Still abstract-only: CALGB 30610.

## Adding a paper

Append a dated `// ===== vN additions =====` block to `data.js` with `TRIALS.<KEY>`, then `OUT` / `LC` / `TOX` / `CONSTRAINTS` rows;
run `node -e "eval(require('fs').readFileSync('data.js','utf8')+';console.log(OUT.length)')"` to validate; commit and push.
