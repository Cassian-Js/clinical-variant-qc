# Clinical Variant QC & Reporting 🧬

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
![Demo](https://img.shields.io/badge/Data-synthetic-orange)
![Purpose](https://img.shields.io/badge/Use-educational_only-red)

**A reproducible introductory bioinformatics project focused on technical QC of small variants from a single-sample VCF.**

> **Not for diagnostic or clinical use.** All coordinates, gene labels, and sample information are synthetic. Coordinates are not mapped to a real genome assembly. QC thresholds are illustrative, not clinically validated. This program does **not** annotate pathogenicity or classify variants under ACMG/AMP.

## What this demonstrates

- Reading VCFv4.2 records with Python's standard library
- Parsing sample `GT`, `DP`, and `GQ` plus site-level `QUAL` and `FILTER`
- Flagging low-depth, low-genotype-quality, low-site-quality and missing genotypes
- Basic SNV/insertion/deletion counting
- Exporting per-variant QC CSV, Markdown report, and SVG graphic
- Reproducibility through unit tests and GitHub Actions

## Quick start

```bash
git clone https://github.com/YOUR_USERNAME/clinical-variant-qc.git
cd clinical-variant-qc
python -m unittest discover -s tests -v
python -m src.pipeline --input data/demo.vcf --output results
```

Python 3.10+; no third-party dependencies required.

To change illustrative thresholds:

```bash
python -m src.pipeline --min-dp 15 --min-gq 25 --min-qual 40
```

## Demo results

10 synthetic records → **5 pass QC**, **5 fail QC** with the default settings.

![QC results](results/qc_summary.svg)

See [`results/report.md`](results/report.md) and [`results/variant_qc.csv`](results/variant_qc.csv).

## How technical QC works

A call passes if `FILTER` is `PASS` (or `.`), genotype is complete, `DP >= 10`, `GQ >= 20`, and `QUAL >= 30`. Missing values fail QC. The `.` FILTER value is treated as not flagged, not proof that site filtering passed; a production workflow should define a stricter policy. All cutoffs are tutorial examples. An adequate call remains **clinically uninterpreted**.

## Data provenance

`data/demo.vcf` was manually authored for training. It contains **no patient records, no real pathogenicity annotations, and no actual reference-genome coordinates**. Gene labels `GENE_A` etc. are placeholders.

## Scope and limitations

- This is an educational parser, not a complete VCF parser. One sample and biallelic non-symbolic SNVs/small indels only.
- Not assessed against benchmark truth sets; no validation of reference alleles or read-level alignment.
- No copy number variants, somatic-specific interpretation, multiallelic splitting, phasing interpretation or clinical reporting.
- Real workflows require reference build verification, normalization, transcript-aware annotation, reviewed ClinVar/ClinGen evidence, pipeline validation, and oversight by qualified professionals.

## Technical references

- [VCF specification (GA4GH / hts-specs)](https://github.com/samtools/hts-specs)
- [NCBI ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/intro/)
- [ClinGen Variant Classification Guidance](https://www.clinicalgenome.org/tools/clingen-variant-classification-guidance/)

## Next iterations

1. Use a validated VCF library such as `pysam` for more complete VCF support.
2. Add assembly-aware, carefully verified annotation using public ClinVar release data.
3. Create a validation set of VCF edge cases, with explicit expected outcomes.
4. Package as an installable CLI and container image.
