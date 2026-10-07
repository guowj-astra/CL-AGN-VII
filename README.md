# CL-AGN VII: data and catalogue-level analysis code

Supporting products for **Changing-look Active Galactic Nuclei from the Dark
Energy Spectroscopic Instrument. VII. Testing Disk--Corona Diagnostics with
eROSITA**, Wei-Jian Guo, Shuo Zhai, Zhu-Heng Yao, Huaqing Cheng, Jingwei Hu and
Jun-Jie Jin. Accepted by MNRAS on 2026 October 5.

**Version v1.0.0**, released 2026 October 7. Download the complete bundle from
[the versioned release](https://github.com/guowj-astra/CL-AGN-VII/releases/tag/v1.0.0).
The archive contains the data, selected analysis/plotting code, checksums and
nine final figure PDFs. A GitHub release URL is supplied; no DOI has been assigned.

## Frozen selection and scope

The accepted paper uses a **10 arcsec** primary match radius. The control
parent has 3,511 unique DESI TARGETID/eRASS UID pairs at strict
`0.35 < z < 0.45`, selected from 4,300 candidate match rows. The main optical
comparison contains 2,032 broad-Halpha AGN (Class A) and 410 no-broad-Halpha
BPT AGN/composite objects (Class B). The Class A P3-quality subset has 1,200
objects. The historical full-redshift CL-AGN table retains 103 records, of
which 89 meet the final 10 arcsec match limit and 78 have finite plotted quantities.
These 78 objects
form an overview, not a like-for-like control
comparison. The independent P1-P5 Class A shape sample contains 1,695 objects;
the adopted P3-P5 hard-band subset has 228 objects after excluding zero-boundary
fits. The 7 arcsec products are **radius-only sensitivity checks**, not the
primary sample. Stellar and emission-line fits were not repeated for this release.

The catalogue-band shape index is exploratory and is **not** a response-aware
spectroscopic photon-index measurement. Use only non-overlapping P1-P5 bands:
P1 0.2-0.5, P2 0.5-1, P3 1-2, P4 2-5, P5 5-8 keV. P6 must not be counted
as an additional independent datum. Fixed luminosity conversions use
`Gamma_X = 1.8`; conditional constant-shape simulations use true shape 1.9.
Those parameters have different roles and are not interchangeable.

## Products and provenance

Paths inside the archive preserve the analysis directory structure.

| Product | Archive location | Role |
| --- | --- | --- |
| Frozen 3,511-row catalogue | `outputs/revision_2026-09-13/M1_M4_original_overleaf/aligned_tests/inputs/comparison_catalogue.csv` | Saved measurements, errors, adopted masses and luminosities |
| Candidate matches and full CL-AGN | `outputs/revision_2026-09-13/R10_wide/inputs/` | Match identities, separations, redshifts and eRASS band values |
| Exact Figure 1 rows | `outputs/desipecfit_clagn_erass1_103_combined/recomputed_mbh_edd_plots/clagn10arcsec_alphaox_priority_pband_marginal_hist_source_data.csv` | 78-object context display |
| Exact retained Figure 2 rows | `outputs/revision_2026-09-13/M1_M4_original_overleaf/qa/figure2_retained_source_rows.csv` | Class A/B plotted source estimates |
| P3/P4 tests | `outputs/revision_2026-09-13/M1_M4_original_overleaf/aligned_tests/M1/` | Paired rows, selection audits and bootstrap draws |
| Independent shape tests | `outputs/revision_2026-09-13/M1_M4_original_overleaf/aligned_tests/M2/` | 1,695 fits, hard selection, bootstrap and conditional null outputs |
| Optical-proxy tests | `outputs/revision_2026-09-13/M1_M4_original_overleaf/aligned_tests/M4/` | 2,687 calibration rows, same-410 Class B variants and bootstrap outputs |
| Bolometric sensitivity | `outputs/revision_2026-09-13/M1_M4_original_overleaf/aligned_tests/science/kX_sensitivity.csv` | Fixed kX = 10, 20, 30 |
| Radius sensitivity | `outputs/revision_2026-09-13/M1_M4_original_overleaf/radius7_increment/` | Exact 10/7 arcsec memberships and point-estimate statistics |
| Historical main-figure products | `archive/legacy_outputs/pre_A01_fix/derived_alphaox_three_samples/` | Explicitly frozen historical outputs; not all columns use the later aligned mass convention |
| Accepted Table 2 transcription | `metadata/accepted_table2.csv` | Values transcribed from the accepted manuscript, not a new regression |
| Final figure PDFs | `figures/` | Exact accepted display layouts, with font-only First Look corrections |

**Important version boundary:** the accepted main figures and five-model
Table 2 were retained, while the added sensitivity tests had their optical
axes aligned to the accepted Class A mass prescription
`logMBH = 6.91 + 0.5*(logL5100 - 44) + 2*log10(FWHM_Halpha/1000)`.
The alignment manifest records that scope. Columns beginning `prior_GH_`
are historical values, not the adopted Class A estimates. The later
seven-model regression script is provided as archived analysis code and must
**not** be described as reproducing the accepted five-model Table 2. Frozen
figure PDFs are the exact visual record. Running historical plotting scripts
with default inputs is not an exact reconstruction of all final figures.

## Units and interpretation

- `TARGETID` and `ERASS_UID` are identifiers; preserve their integer values.
- `Z`/`z` is dimensionless; separations ending `ARCSEC`/`arcsec` are arcseconds.
- eRASS `ML_FLUX_P*` and errors are integrated energy fluxes in erg s^-1 cm^-2;
  `DET_LIKE_P*` is catalogue detection likelihood, not flux signal-to-noise.
- DESI integrated line flux columns are in units of 10^-17 erg s^-1 cm^-2;
  explicit column suffixes take precedence. Do not confuse line flux with
  flux density per Angstrom.
- FWHM and stellar sigma columns ending `kms` are km s^-1; mass logs are
  log10(MBH/Msun), luminosities ending `erg_s` are log10(erg s^-1), and
  `logLnu` columns are log10(erg s^-1 Hz^-1).
- Eddington ratios, alphaOX and shape indices are dimensionless; logarithms
  are base 10. Larger alphaOX means relatively weaker X-rays.
- `NaN`/empty numeric values mean unavailable, not zero. Retain saved quality
  flags and selection columns. Published fit uncertainties do not include
  every systematic uncertainty in optical proxies or the virial mass scale.
- [O III] scaling is an empirical internal luminosity calibration, not a
  universal physical bolometric correction. Class B is a diagnostic control.

## Verification and analysis

Use Python 3.11 or later and the packages in `requirements.txt`. From the
extracted archive root:

```sh
python verify_release.py
python -m unittest discover -s tests
CLAGN_MATCH_RADIUS_ARCSEC=10 CLAGN_ALPHA_SELECTION=wide CLAGN_SHAPE_SELECTION=independent python scripts/check_accepted_overleaf_radius.py
```

The first check validates checksums, unique identities, sample counts,
redshift range, P3 coverage of rest-frame 2 keV, adopted mass/ratio algebra,
and correspondence between shape and optical rows. The radius check writes
fresh descriptive outputs; it does not redo pPXF, line fitting or Monte Carlo.
`replay_appendix.py M1` (or `M2`, `M4`) explicitly points to released inputs
and writes to `replay_outputs/`, leaving the frozen products untouched.
Full bootstrap/null replays can be computationally expensive. They are
provided as optional reruns; numerical libraries/platforms can change draws
or floating-point details. Not every historical plotting/default path is
part of this supported replay.
For the original draw counts use `--draws 5000` for M1/M4, and
`--draws 1000 --bootstrap 5000` for M2. Low-draw runs are smoke tests only.

The release provides catalogue-level analysis code, not a turnkey DESI
spectral fitting installation or an eROSITA response/count-spectrum pipeline.
It does not include raw spectra, proprietary data, server credentials,
manuscript correspondence, review documents or previous rejected samples.
No blanket software/data reuse licence is assigned by this release; upstream
survey terms continue to apply. Please cite the paper and this versioned
repository when using these supporting products.

## Public survey access

- [DESI DR1](https://data.desi.lbl.gov/doc/releases/dr1/)
- [eROSITA-DE DR1 eRASS1 catalogues](https://erosita.mpe.mpg.de/dr1/AllSkySurveyData_dr1/Catalogues_dr1/)

`manifest.json` lists every released source product, its relative provenance,
SHA-256 hash, byte size, and CSV row/column inventory. JSON provenance paths
were made relative; numerical table files were not changed.
