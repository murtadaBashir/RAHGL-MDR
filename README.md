# RAHGL-MDR
# RAHGL-MDR

## Relation-Aware Heterogeneous Graph Learning for miRNA–Drug Resistance Prediction under Cold-Start Generalization

RAHGL-MDR is a heterogeneous graph learning framework for predicting miRNA–drug resistance associations. The framework integrates biological, pharmacological, and molecular information into a unified miRNA–gene–drug network and evaluates prediction performance under both conventional edge-based and more challenging cold-start settings.

## Overview

The model uses a Heterogeneous Graph Transformer (HGT) to preserve the different biological meanings of relations within the heterogeneous network. In addition to the HGT representation, pair-specific biological paths are used to provide contextual information for candidate miRNA–drug pairs.

The framework integrates:

- miRNA expression profiles
- miRNA–gene regulatory interactions
- Gene–gene functional associations
- Drug molecular representations
- Drug–target interactions
- Drug–enzyme interactions
- Drug–transporter interactions
- Drug–carrier interactions
- Experimentally supported miRNA–drug resistance associations

## Data Sources

The heterogeneous network was constructed using information obtained from several public biomedical resources:

- **ncRNADrug** — experimentally supported miRNA–drug resistance associations
- **ENCORI (starBase)** — miRNA–gene regulatory interactions
- **DrugBank** — drug–target, drug–enzyme, drug–transporter, and drug–carrier relations
- **HumanNet** — gene–gene functional associations
- **CCLE** — miRNA expression profiles
- **ChemBERTa** — molecular representations derived from drug SMILES

After preprocessing, the resistance dataset contained **5,379 positive miRNA–drug resistance associations involving 635 miRNAs and 216 drugs**.

## Model

RAHGL-MDR combines several sources of information for prediction:

1. **Relation-aware heterogeneous graph learning** using HGT.
2. **Pair-specific biological paths** connecting miRNAs and drugs through genes.
3. **Drug molecular representations** generated using ChemBERTa.
4. **Adaptive fusion** of graph and pair-specific information.

The different DrugBank relation types are retained separately rather than being collapsed into a single generic drug–gene relation.

## Evaluation

The model is evaluated using leakage-safe five-fold protocols under four settings:

- **Edge-based** — individual miRNA–drug associations are held out.
- **miRNA-cold** — test miRNAs have no resistance associations in training.
- **Drug-cold** — test drugs have no resistance associations in training.
- **Double-cold** — both the miRNA and drug in a test pair are unseen with respect to resistance supervision.

Validation and test resistance edges are excluded from the fold-specific message-passing graph to prevent information leakage.

## Main Results

| Evaluation Setting | AUC | AUPR |
|--------------------|-----|------|
| Edge-based | 0.9533 ± 0.0040 | 0.9492 ± 0.0069 |
| miRNA-cold | 0.9466 ± 0.0050 | 0.9435 ± 0.0039 |
| Drug-cold | 0.6615 ± 0.0323 | 0.6468 ± 0.0386 |
| Double-cold | 0.6348 ± 0.0869 | 0.6412 ± 0.1056 |

The results show that the model retains strong predictive performance for unseen miRNAs. Generalization to previously unseen drugs is considerably more difficult, highlighting an important limitation that may not be apparent from conventional edge-based evaluation alone.

## Biological Validation

Independent literature evidence was used to examine high-confidence predictions that were not included in the resistance labels.

Among the **20 highest-ranked previously unlabeled miRNA–drug pairs, 6 were supported by independent literature evidence**, compared with **0 of 20 randomly selected unlabeled control pairs**.

The enrichment was statistically significant using a one-sided Fisher's exact test (**p = 0.0101**).

## Repository Structure

```text
RAHGL-MDR/
├── Code/
│   └── Model training and evaluation scripts
├── Data/
│   └── Processed data used by the framework
├── Results/
│   └── Model predictions and evaluation results
├── Figures/
│   └── Figures generated from the experiments
└── README.md
