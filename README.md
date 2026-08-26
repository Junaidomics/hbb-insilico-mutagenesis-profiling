# HBB In-Silico Mutagenesis & Structural Profiling

### Comparative sequence, physicochemical, and structural analysis of HbA, HbS, and HbC


> A reproducible computational analysis of the clinically important **HBB Glu6Val (HbS)** and **HBB Glu6Lys (HbC)** substitutions using controlled sequence mutagenesis, protein translation, physicochemical profiling, and wild-type structural-context analysis.



## Overview

This project investigates how two different missense substitutions at the same position in the human **β-globin (*****HBB*****) protein** produce markedly different physicochemical changes.

The analysis compares:

| Variant | Mutation  | Mature residue 6 | Primary physicochemical shift     |
| ------- | --------- | ---------------: | --------------------------------- |
| **HbA** | Reference |          Glu (E) | Baseline                          |
| **HbS** | Glu → Val |          Val (V) | Strong increase in hydrophobicity |
| **HbC** | Glu → Lys |          Lys (K) | Charge inversion                  |

The computational workflow starts from a human *HBB* coding sequence, introduces controlled codon substitutions, translates the resulting sequences, quantifies residue-level physicochemical changes, and examines the native spatial neighborhood of Glu6 in the deoxyhemoglobin structure **PDB 1A3N**.

The central question is:

> **How can substitutions at the same mature β-globin residue generate different physicochemical perturbations, and what does the native structural environment around Glu6 reveal about those differences?**



## Biological Context

The human **β-globin (*****HBB*****)** protein is a component of adult hemoglobin.

Two well-known pathogenic substitutions occur at mature residue 6:

* **HbS:** Glu6Val (E6V)
* **HbC:** Glu6Lys (E6K)

Although both mutations affect the same residue, they replace glutamate with chemically very different amino acids.

This project therefore uses the two variants as a compact case study for connecting:

**DNA sequence → amino-acid substitution → physicochemical change → structural context → biological interpretation**

The project focuses on computational characterization rather than clinical diagnosis or experimental validation.

---

## Project Objectives

### Primary objectives

1. Validate the input human *HBB* coding sequence.
2. Introduce controlled codon substitutions corresponding to HbS and HbC.
3. Translate the reference and mutant CDS sequences.
4. Verify the expected mature residue at position 6.
5. Quantify physicochemical changes using:

   * Kyte–Doolittle hydropathy
   * simplified formal side-chain charge
   * Grantham chemical distance
6. Analyze the native 3D neighborhood surrounding Glu6 in PDB **1A3N**.
7. Integrate sequence-level and structural observations into a biologically interpretable comparison.
8. Maintain explicit methodological boundaries so that structural observations are not overstated as mutant simulations.

---

# Workflow

```text
                  Human HBB CDS
                       │
                       ▼
              Sequence Quality Control
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
            HbA              Targeted mutation
                                │
                       ┌────────┴────────┐
                       ▼                 ▼
                     HbS               HbC
                    E → V             E → K
                       │                 │
                       └────────┬────────┘
                                ▼
                       CDS Translation
                                │
                                ▼
                    Mature β-globin chains
                                │
                                ▼
              Physicochemical Characterization
             ┌──────────┬──────────┬──────────┐
             ▼          ▼          ▼
         Hydropathy   Charge   Grantham distance
             │          │          │
             └──────────┴──────────┘
                        │
                        ▼
              Native Structural Context
                     PDB 1A3N
                        │
                        ▼
                 Chain B / Glu6
                        │
                        ▼
             Cα neighborhood ≤ 8 Å
                        │
                        ▼
              Comparative Interpretation
```

---

# Dataset & Structural Input

## HBB coding sequence

The project uses the provided human *HBB* coding sequence:

```text
Accession: CR536530.1
Length: 444 nt
```

The sequence is validated for:

* expected accession
* canonical DNA alphabet
* CDS length divisible by three
* ATG start codon
* canonical stop codon
* expected reference codon at the target position

The validated target codon is:

```text
Codon 7: GAG
```

Because the initiator methionine is removed during maturation, the project maps:

```text
CDS codon 7
      ↓
Mature β-globin residue 6
```

This coordinate distinction is explicitly handled in the notebook.

---

## Structural model

The structural analysis uses:

```text
PDB: 1A3N
```

The workflow extracts **Chain B** and identifies native Glu6.

The analysis then calculates Cα-to-Cα distances between Glu6 and other residues within an **8 Å radius**.

Importantly, this is a **wild-type structural-context analysis**.

The project does **not** generate experimentally validated or energy-minimized mutant structures.

---

# In-Silico Mutagenesis

The reference codon is:

```text
HbA
GAG
 ↓
Glu (E)
```

Two controlled substitutions are generated:

```text
HbS
GTG
 ↓
Val (V)

HbC
AAG
 ↓
Lys (K)
```

The resulting translated mature chains are checked programmatically.

Expected result:

| Variant | Codon 7 | Mature residue 6 | Mature chain length |
| ------- | ------- | ---------------- | ------------------: |
| HbA     | GAG     | E                |              146 aa |
| HbS     | GTG     | V                |              146 aa |
| HbC     | AAG     | K                |              146 aa |

These assertions are implemented directly in the notebook to prevent silent sequence/coordinate errors.

---

# Physicochemical Analysis

Three residue-level descriptors are calculated.

## 1. Kyte–Doolittle hydropathy

The analysis uses the Kyte–Doolittle hydropathy scale to quantify the relative hydrophobic character of the substituted residue.

Observed values:

| Variant | Residue | Hydropathy | Δ Hydropathy vs HbA |
| ------- | ------- | ---------: | ------------------: |
| HbA     | E       |       -3.5 |                 0.0 |
| HbS     | V       |        4.2 |                +7.7 |
| HbC     | K       |       -3.9 |                -0.4 |

The E→V substitution therefore produces a large hydropathy increase, whereas E→K does not.

---

## 2. Simplified formal charge

The notebook represents side-chain formal charge near physiological pH using a simplified residue-level scheme.

| Variant | Residue | Formal charge | Δ Charge vs HbA |
| ------- | ------- | ------------: | --------------: |
| HbA     | E       |            -1 |               0 |
| HbS     | V       |             0 |              +1 |
| HbC     | K       |            +1 |              +2 |

The E→K substitution therefore produces the largest charge perturbation in this simplified representation.

> **Important:** These are simplified formal side-chain states, not residue-specific electrostatic calculations or experimentally measured pKa values.

---

## 3. Grantham distance

Grantham distance is used as a measure of chemical dissimilarity between the reference and substituted amino acids.

| Substitution | Grantham distance |
| ------------ | ----------------: |
| E → E        |                 0 |
| E → V        |               121 |
| E → K        |                56 |

In this analysis, E→V is chemically more dissimilar according to the Grantham metric than E→K.

---

# Structural Analysis

The project examines the native environment of Glu6 using the deoxyhemoglobin structure **1A3N**.

### Analysis parameters

```text
Structure:        1A3N
Chain:            B
Target residue:   Glu6
Atom used:        Cα
Neighborhood:     ≤ 8.0 Å
```

The calculated native neighborhood contains seven residues within the defined Cα distance threshold:

| Neighbor | Residue | Cα distance |
| -------- | ------- | ----------: |
| P5       | Pro     |      3.81 Å |
| E7       | Glu     |      3.86 Å |
| S9       | Ser     |      5.25 Å |
| T4       | Thr     |      5.44 Å |
| K8       | Lys     |      5.67 Å |
| A10      | Ala     |      6.08 Å |
| L3       | Leu     |      7.99 Å |

This provides a quantitative description of the local structural environment surrounding native Glu6.

---

# Key Computational Findings

### HbS — E6V

The E→V substitution produces:

* **Δ hydropathy = +7.7**
* **Δ formal charge = +1**
* **Grantham distance = 121**

The dominant computational signal is therefore the large increase in hydrophobicity and chemical dissimilarity.

### HbC — E6K

The E→K substitution produces:

* **Δ hydropathy = −0.4**
* **Δ formal charge = +2**
* **Grantham distance = 56**

The dominant computational signal is therefore the change in formal charge rather than an increase in hydrophobicity.

---

# Biological Interpretation

The analysis supports a useful mechanistic distinction:

```text
                 Native Glu6
                     │
             ┌───────┴────────┐
             │                │
          E → V            E → K
           HbS               HbC
             │                │
             ▼                ▼
      Strong hydropathy     Charge inversion
          increase             (+2)
             │                │
             ▼                ▼
       Altered surface      Altered surface
       physicochemical      electrostatic
          character           character
```

The results are consistent with the well-established concept that **different amino-acid substitutions at the same protein position can produce qualitatively different molecular perturbations**.

For HbS, the large E→V hydropathy shift provides a straightforward physicochemical basis for discussing hydrophobic surface changes associated with sickling polymer formation.

For HbC, the E→K substitution produces a strong charge reversal without a comparable hydrophobicity increase, supporting a distinct physicochemical interpretation.

However, this notebook does **not** calculate polymerization energies, crystal nucleation energies, mutant binding affinities, molecular dynamics trajectories, or mutant protein stability.

Therefore, mechanistic statements beyond the calculated sequence and structural descriptors should be treated as **biological interpretation supported by established hemoglobin biology**, rather than direct predictions from this pipeline.

---

# Methodological Boundary

This distinction is central to the scientific interpretation of the project.

### What this project DOES

* Performs controlled sequence-level mutagenesis.
* Translates reference and mutant CDS sequences.
* Verifies amino-acid identities.
* Quantifies residue-level physicochemical changes.
* Calculates Cα spatial proximity in a wild-type structure.
* Provides a reproducible computational comparison of HbA, HbS, and HbC.

### What this project DOES NOT

* Predict clinical severity.
* Perform molecular dynamics simulations.
* Relax or energy-minimize mutant structures.
* Calculate mutant folding free energies.
* Calculate protein–protein binding energies.
* Simulate HbS polymerization.
* Simulate HbC crystallization.
* Perform molecular docking.
* Replace experimental structural or clinical evidence.

This separation between **calculated results** and **biological interpretation** is intentional.

---

# Reproducibility

The repository is designed around a small, reproducible input → analysis → interpretation workflow.

Recommended repository structure:

```text
hbb-insilico-mutagenesis-profiling/
│
├── README.md
├── hbb_variant_analysis.ipynb
└── human_hbb_cds.fasta
└── 1A3N.pdb
```

---

# Installation

Clone the repository:

```bash
git clone https://github.com/Junaidomics/hbb-insilico-mutagenesis-profiling.git
cd hbb-insilico-mutagenesis-profiling
```

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it on Linux/macOS:

```bash
source .venv/bin/activate
```

On Windows:

```bash
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Launch Jupyter:

```bash
jupyter notebook
```

Then open:

```text
hbb_variant_analysis.ipynb
```

---

# Core Dependencies

The notebook uses:

* **Python**
* **Biopython**

  * FASTA parsing
  * CDS translation
  * PDB parsing
* **pandas**

  * tabular analysis
* **matplotlib**

  * visualization
* **Jupyter Notebook**

  * reproducible analysis environment

A minimal `requirements.txt` can contain:

```text
biopython
pandas
matplotlib
jupyter
```

For strict reproducibility, dependency versions should be pinned in a future release.

---

# Reproducibility Checks

The notebook contains explicit assertions for key biological and computational assumptions.

Examples include:

```python
assert record.id == "CR536530.1"
assert set(wt_cds) <= set("ACGT")
assert len(wt_cds) % 3 == 0
assert wt_cds.startswith("ATG")
assert wt_cds[-3:] in {"TAA", "TAG", "TGA"}
```

Variant identity is also verified:

```python
assert mature_chains["HbA"][5] == "E"
assert mature_chains["HbS"][5] == "V"
assert mature_chains["HbC"][5] == "K"
```

And the expected mature chain length is checked:

```python
assert all(len(seq) == 146 for seq in mature_chains.values())
```

These checks help catch common bioinformatics errors involving:

* incorrect sequence files
* frame shifts
* wrong codon coordinates
* incorrect residue numbering
* unexpected sequence lengths

---

# Visualization

The notebook generates a comparative figure containing two analytical panels:

### Panel A — Physicochemical shifts

Compares:

* Δ hydropathy
* Δ formal charge

for HbA, HbS, and HbC.

### Panel B — Structural neighborhood

Displays Cα distances from native Glu6 to neighboring residues in Chain B of PDB 1A3N, using 8 Å as the analysis threshold.

---

# Limitations

Several limitations should be considered when interpreting the results.

### 1. Static wild-type structure

The structural analysis uses wild-type coordinates from **1A3N**.

Mutant side-chain rearrangements and backbone relaxation are not modeled.

### 2. Cα distance is not molecular contact

An 8 Å Cα distance is a geometric neighborhood criterion.

It does not directly quantify:

* atomic contacts
* hydrogen bonds
* salt bridges
* van der Waals interactions
* solvent-accessible surface area
* binding interfaces

### 3. Simplified charge model

The charge analysis uses simplified residue-level formal charges.

It does not calculate:

* local pKa shifts
* protonation-state equilibria
* electrostatic potential
* solvent effects

### 4. No mutant structural modeling

The project does not generate independent HbS or HbC 3D structures.

Consequently, structural conclusions are based on the **native Glu6 environment**, not simulated mutant conformations.

### 5. No direct polymerization/crystallization simulation

The project provides physicochemical and structural context but does not quantitatively model HbS polymerization or HbC crystallization.

---

# Why This Project Matters

This project demonstrates a compact example of how computational biology can connect multiple analytical levels:

```text
Nucleotide sequence
        ↓
Codon-level mutation
        ↓
Protein translation
        ↓
Amino-acid properties
        ↓
3D structural context
        ↓
Biological interpretation
```

Rather than treating a mutation as simply:

```text
E6V
```

the workflow asks:

> **What actually changes when E6 is replaced by V or K?**

That question can be decomposed computationally into measurable properties such as hydropathy, charge, chemical distance, and spatial context.

This makes the project useful as a portfolio example of **sequence analysis + structural bioinformatics + reproducible Python-based analysis**.

---

# Future Development

Potential extensions include:

* [ ] Add automated retrieval and provenance tracking for sequence/structure records.
* [ ] Add solvent-accessible surface area calculations.
* [ ] Add residue-level electrostatic analysis.
* [ ] Add side-chain contact analysis instead of Cα-only proximity.
* [ ] Generate modeled HbS and HbC structures.
* [ ] Perform structural relaxation of mutant side chains.
* [ ] Compare predicted structural changes using multiple conformations.
* [ ] Add molecular dynamics simulations.
* [ ] Quantify mutant stability using appropriate computational methods.
* [ ] Add automated visualization with PyMOL or ChimeraX.
* [ ] Add unit tests for the mutation and coordinate-mapping functions.
* [ ] Add a reproducible environment specification.
* [ ] Add automated CI testing with GitHub Actions.

---

# Scientific Scope

This repository is an **educational/research bioinformatics project** demonstrating computational analysis of HBB variants.

It is not intended to provide:

* clinical diagnosis
* individual patient interpretation
* medical advice
* clinical variant classification
* therapeutic recommendations

Computational observations should be interpreted alongside experimental and clinical evidence.

---

# Data Provenance

## HBB sequence

The input FASTA supplied with this project identifies:

```text
CR536530.1
Homo sapiens HBB full open reading frame cDNA clone
```

The sequence is stored locally in:

```text
data/human_hbb_cds.fasta
```

## Protein structure

The structural input is:

```text
PDB ID: 1A3N
```

and is stored locally as:

```text
data/1A3N.pdb
```

Users should consult the corresponding primary database records for the authoritative metadata and experimental details associated with these resources.

---

# Author

**Muhammad Junaid**

Bioinformatics | Biotechnology | Computational Biology

GitHub: **[@Junaidomics](https://github.com/Junaidomics)**

Project: **[hbb-insilico-mutagenesis-profiling](https://github.com/Junaidomics/hbb-insilico-mutagenesis-profiling)**

---

# Citation

If you use or adapt this project, please cite the repository:

```text
Junaid, M. HBB In-Silico Mutagenesis & Structural Profiling.
GitHub repository:
https://github.com/Junaidomics/hbb-insilico-mutagenesis-profiling
```

For scientific work, also cite the original databases and literature associated with the HBB sequence, hemoglobin structures, and HbS/HbC biology rather than citing this repository as the primary source for established biological facts.

---

## Project Status

**Status:** Research / Portfolio Project

The current implementation focuses on targeted computational comparison of:

```text
HbA  →  HbS
HbA  →  HbC
```

at mature β-globin residue 6.

The workflow is intentionally transparent and interpretable, with explicit quality-control checks and stated methodological limitations.
