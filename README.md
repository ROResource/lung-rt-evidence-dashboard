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
`scrt` sequential CRT · `ccrt` concurrent CRT · `crtio` CRT + IO · `port` · `pci` · comparators `surg`, `sys`, `none`.

## Conventions (same as the endometrial dashboard)

- Only author-reported numbers, transcribed from the full text (pdftotext) of the PDF library. Where the full text was not
  available the trial is tagged `src:"abs"` (published abstract) or `src:"rev"` (secondary review) and shown with a tag on the site.
- Nothing is pooled. "Typical range" on the Overview = min–max of arm-level point estimates for that treatment type within the group.
- Toxicity rows state grade threshold, acute/late and grading system; grades are never merged. Derived values (e.g. 100 − LC) are labelled.
- DOIs were verified against Crossref; where none was found `doi:""` and a PubMed search term (`pm`) is used.
- No PDFs or spreadsheets are committed.

## Adding a paper

Append a dated `// ===== vN additions =====` block to `data.js` with `TRIALS.<KEY>`, then `OUT` / `LC` / `TOX` / `CONSTRAINTS` rows;
run `node -e "eval(require('fs').readFileSync('data.js','utf8')+';console.log(OUT.length)')"` to validate; commit and push.
