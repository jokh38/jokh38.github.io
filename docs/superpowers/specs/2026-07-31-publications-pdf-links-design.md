# Publications page: expose downloadable PDFs

## Problem

The user exported their publication PDFs from Zotero into `pub_files/내보낸 항목/files/<id>/*.pdf` (26 PDF files). The site's `_publications` collection has 31 entries, rendered on `/publications/` via `_includes/archive-single-publication.html` as an expandable accordion (title → abstract + "View on PubMed" link). There is currently no way to download the actual PDF from the publications page.

The underlying Jekyll theme (academicpages) already supports a `paperurl` frontmatter field and renders a "Download Paper" link for it, but only in `_includes/archive-single.html` (the single-post permalink page template). The customized listing include used on `/publications/`, `archive-single-publication.html`, does not check `paperurl` at all — so even setting the field would have no visible effect on the listing page users actually browse.

## Goals

- Clicking into a publication's abstract on `/publications/` shows a "Download PDF" link when a PDF is available.
- PDFs are served as static files from the Jekyll site, not left in the Zotero export folder.
- Publications without a matched PDF render exactly as they do today (no broken links).

## Non-goals

- Reorganizing or deleting `pub_files/` (kept as the user's Zotero source/reference).
- Automated/fuzzy matching tooling — this is a one-time mapping of 26 files done by hand.
- Sourcing the 5 missing PDFs (reported to the user as a follow-up item).

## Design

### 1. File placement

Copy the 26 matched PDFs into `files/papers/`, alongside the theme's existing `files/` folder (which already holds sample `paper1.pdf`, `slides1.pdf`, etc.). Rename each to the corresponding publication's slug for a predictable, ASCII-safe name that obviously ties back to its post, e.g.:

```
files/papers/2015-proton-liver-sparing-hcc.pdf
files/papers/2022-gevi-range-verification-pbs.pdf
```

Slug = the `_publications/*.md` filename with the leading `YYYY-MM-DD-` date stripped (this matches the second half of each file's existing `permalink:` value).

### 2. Frontmatter

Add `paperurl: "/files/papers/<slug>.pdf"` to each of the 26 matched `_publications/*.md` files. The 5 unmatched entries are left untouched (no `paperurl` key).

### 3. Template change

In `_includes/archive-single-publication.html`, inside the existing `.publication-abstract` block, add a conditional link directly after the "View on PubMed" line:

```liquid
{% if post.paperurl %}
<p><a href="{{ post.paperurl | relative_url }}" target="_blank" rel="noopener"><i class="fa fa-file-pdf-o" aria-hidden="true"></i> Download PDF</a></p>
{% endif %}
```

Entries without `paperurl` render unchanged.

### 4. Publication ↔ PDF mapping (26 matched)

| Publication slug | Source PDF (under `pub_files/내보낸 항목/files/`) |
|---|---|
| 2008-vector-susceptibility-qcd-ads | `3187/Jo 등 - 2008 - Vector susceptibility and QCD phase transition in AdSQCD Models.pdf` |
| 2010-holographic-qcd-medium | `3188/Jo 등 - 2010 - Holographic QCD in medium a bottom up approach.pdf` |
| 2011-photoemission-qgp-finite-density | `3192/Jo 및 Sin - 2011 - Photoemission rate of strongly interacting quark-gluon plasma at finite density.pdf` |
| 2011-rn-ads4-hydrodynamics-holographic-optics | `3189/Ge 등 - 2011 - Hydrodynamics of RN AdS4 black hole and holographic optics.pdf` |
| 2011-holographic-mesons-d4-d6 | `3190/Jo 등 - 2011 - Holographic mesons in D4D6 model revisited.pdf` |
| 2011-in-medium-hadron-mass-drop | `3191/Jo 등 - 2011 - The dropping of in-medium hadron mass in holographic QCD.pdf` |
| 2015-proton-liver-sparing-hcc | `3198/Kim 등 - 2015 - Normal liver sparing by proton beam therapy for hepatocellular carcinoma Comparison with helical in.pdf` |
| 2015-dose-modeling-pbs-kncc | `3197/Jo 등 - 2015 - A Practical Experience of Dose Modeling for Proton Pencil Beam Scanning in KNCC.pdf` |
| 2015-smc-proton-center-status | `3196/Chung 등 - 2015 - The first private-hospital based proton therapy center in korea; status of the proton therapy center.pdf` |
| 2016-patient-setup-laser-alignment | `3199/Hwang 등 - 2016 - A new method and device of aligning patient setup lasers in radiation therapy.pdf` |
| 2016-treatment-delay-times-smc-ptc | `3200/Jo 등 - 2016 - Analysis of Treatment and Delay Times by Disease Site and Delivery Technique at Samsung Medical Cent.pdf` |
| 2017-proton-radiography-water-range-shifter | `3202/Park 등 - 2017 - Proton radiography and computed tomography with dynamic water range shifter.pdf` |
| 2017-fluka-secondary-neutron-scanning-proton | `3201/Lee 등 - 2017 - Monte Carlo simulation of secondary neutron dose for scanning proton therapy using FLUKA.pdf` |
| 2017-lung-sparing-tomotherapy-stage3-nsclc | `3425/Hong 등 - 2017 - Normal lung sparing Tomotherapy technique in stage III lung cancer.pdf` |
| 2018-in-vivo-rectal-dosimetry-hdr | `3203/Jeang 등 - 2018 - Two-dimensional in vivo rectal dosimetry during high-dose-rate brachytherapy for cervical can.pdf` |
| 2018-proton-range-verification-inhomogeneous | `3204/Kim 등 - 2018 - Proton range verification in inhomogeneous tissue Treatment planning system vs. measurement vs. Mon.pdf` |
| 2018-line-scanning-dmlc-proton | `3205/Kim 등 - 2018 - Investigations of line scanning proton therapy with dynamic multi-leaf collimator.pdf` |
| 2019-tomotherapy-impt-nasopharynx | `3207/Park 등 - 2019 - Early clinical outcomes of helical tomotherapy  intensity ‐ modulated proton therapy combination in.pdf` |
| 2019-cnn-dose-deconvolution | `3206/Cheon 등 - 2019 - Feasibility of two-dimensional dose distribution deconvolution using convolution neural networks.pdf` |
| 2020-qa-isocentres-motion-capture | `3208/Cheon 등 - 2020 - Quality assurance of isocentres for passive proton beam nozzles using motion capture cameras.pdf` |
| 2021-mu-prediction-wobbling-ridge | `3209/Jo 등 - 2021 - Monitor unit prediction model for wobbling proton therapy with ridge filters.pdf` |
| 2022-gevi-range-verification-pbs | `3137/Kim 등 - 2021 - Upgrade of gamma electron vertex imaging system for high-performance range verification in pencil be.pdf` (PDF metadata year 2021 vs. post year 2022; same paper, online-first vs. issue date) |
| 2023-mc-qa-line-scanning-proton | `3541/Jeon 등 - 2023 - Monte Carlo simulation‐based patient‐specific QA using machine log files for line‐scanning proton ra.pdf` |
| 2024-digital-star-shot-isocenters | `3210/Han 및 Jo - 2024 - Development of a digital star‐shot analysis system for comparing radiation and imaging isocenters of.pdf` |
| 2025-beam-on-time-respiratory-gating-liver | `3159/Jo 등 - 2025 - The dependency of beam‐on‐time efficiency on respiration period and layer switching time for respira.pdf` |
| 2026-dynamic-range-optimization-raystation | `3211/Cho 등 - 2026 - Dynamic range optimization for treatment time reduction in respiratory‐gated proton therapy using Ra.pdf` |

### 5. Missing PDFs (5, no `paperurl` added)

- `2006-vector-susceptibility-chiral` — Vector susceptibility and chiral phase transition in AdS/QCD models
- `2019-psqa-wobbling-line-scanning-smc` — Initial experience of patient-specific QA for wobbling and line-scanning proton therapy at Samsung Medical Center
- `2022-spheroid-proton-drug-combinations` — High-throughput 3D tumor spheroid array platform for evaluating sensitivity of proton-drug combinations
- `2022-proton-vs-photon-reirradiation-nsclc` — Toxicity of proton therapy versus photon therapy on salvage re-irradiation for non-small cell lung cancer
- `2024-aperture-mlc-line-scanning` — Utility of a patient-specific aperture collimator and multi-leaf collimator in line scanning proton therapy

To be reported to the user after implementation; no action taken on these in this design.

## Testing

- Run `bundle exec jekyll build` (or serve) and manually check `/publications/`: expand a publication with a PDF and confirm the "Download PDF" link appears and resolves; expand one of the 5 without a PDF and confirm no broken link/no link shown at all.
- Spot-check a couple of PDFs open correctly from the served path.
