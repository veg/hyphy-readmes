# BUSTED-PH: Gene-wide detection of trait-associated positive selection

## Overview
BUSTED-PH (Branch-Site Unrestricted Statistical Test for Episodic Diversification - Phenotype) is a statistical method designed to identify genes where episodic diversifying selection (EDS) is associated with a binary trait (phenotype). It is particularly useful for detecting signals of convergent evolution where multiple independent lineages (species or clades) share a phenotype.

The method explicitly models and contrasts positive selection across two sets of branches in a phylogenetic tree:
1.  **Foreground (FG)**: Branches associated with the phenotype of interest.
2.  **Background (BG)**: Branches not associated with the phenotype.

BUSTED-PH performs three distinct statistical tests to ensure that the detected selection is specific to the trait:
1.  **Test for Selection on FG**: Is there evidence of positive selection ($\omega > 1$) on the foreground branches?
2.  **Test for Selection on BG**: Is there evidence of positive selection on the background branches?
3.  **Test for Difference**: Is the selective regime (distribution of $\omega$ rates) significantly different between FG and BG branches?

Trait-associated positive selection is inferred when there is significant selection on FG, no significant selection on BG, and a significant difference between the two.

## Citation
If you use BUSTED-PH, please cite:

**BUSTED-PH: Gene-wide detection of trait-associated positive selection**
Avery Selberg, Nathan Clark, Maria Chikina, Sergei L Kosakovsky Pond.
*Manuscript in preparation/review.*

## Input Requirements
To run BUSTED-PH, you need:

1.  **Sequence Alignment**: A codon-aware multiple sequence alignment (e.g., FASTA, NEXUS, PHYLIP).
    *   Stop codons must be removed or stripped.
    *   The alignment should be in frame.
2.  **Phylogenetic Tree**: A tree corresponding to the alignment.
    *   The tree must have branches partitioned into **Foreground** (phenotype present) and **Background** (phenotype absent).
    *   This is typically done by labeling the tree string (e.g., `{FG}` tags) or using an annotation file.

## Usage
BUSTED-PH is implemented in HyPhy (version 2.5.73 or later).

### Command Line Interface
```bash
hyphy busted-ph --alignment path/to/alignment.fasta --tree path/to/tree.nwk --branches ForegroundLabel [options]
```

### Key Arguments
| Argument | Description | Default |
| :--- | :--- | :--- |
| `--alignment` | Path to the codon alignment file. | Required |
| `--tree` | Path to the phylogenetic tree file (if not included in alignment). | Required |
| `--branches` | The label in the tree file denoting the Foreground branches. | Required |
| `--rates` | Number of rate classes (k). The paper suggests k=2 or k=3. | 3 |
| `--error-sink` | Enable BUSTED-E style error mitigation (Yes/No). Useful for noisy alignments. | No |
| `--srv` | Enable Synonymous Rate Variation (Yes/No). | Yes |

## Outputs
BUSTED-PH produces a JSON file containing the detailed results of the model fits and hypothesis tests.

### Standard Output
The console will print the progress of the likelihood optimization and a summary of the three likelihood ratio tests (LRTs).

### JSON Output
The primary output is a JSON file (usually named `[alignment].json`).

*   **`test results`**: Contains the p-values for the three main tests:
    *   `FG` (Selection on Foreground)
    *   `BG` (Selection on Background)
    *   `SHARED` (Difference between FG and BG)
*   **`fits`**: Detailed parameter estimates for the different models (Alternative, Nulls).
*   **`rate distributions`**: The inferred distributions of $\omega$ (dN/dS) for FG and BG branches.

## Interpretation
A gene is considered to exhibit **trait-associated episodic diversifying selection (EDS)** if it meets the following criteria:

1.  **Selection on FG**: Significant p-value ($p < 0.05$) for the FG test.
2.  **No Selection on BG**: Non-significant p-value ($p > 0.05$) for the BG test.
3.  **Significant Difference**: Significant p-value ($p < 0.05$) for the Difference test.

**Note**: P-values should be corrected for multiple testing (e.g., Holm-Bonferroni) if analyzing many genes. The "Difference" test is crucial to rule out genes that are under selection across the entire tree (not just the trait lineages).

## LLM-Targeted Information

### Analysis Prompt Template
Use the following prompt to ask an LLM to interpret your BUSTED-PH JSON results:

> "I ran the BUSTED-PH analysis on my dataset to detect positive selection associated with [Trait Name].
>
> Here are the key p-values from the JSON output:
> - **Test for Selection on Foreground (FG)**: [Insert p-value]
> - **Test for Selection on Background (BG)**: [Insert p-value]
> - **Test for Difference (FG vs BG)**: [Insert p-value]
>
> Based on these results, is there evidence for trait-associated positive selection? Explain if the selection is specific to the trait or widespread. Also, look at the `fits` section if available to see the estimated omega values."

### JSON Field Descriptions
*   `test results.p-values.FG`: P-value testing if there is positive selection (omega > 1) specifically on the foreground branches.
*   `test results.p-values.BG`: P-value testing if there is positive selection on the background branches.
*   `test results.p-values.SHARED`: P-value testing if the selective regimes (omega distributions) are statistically different between foreground and background.
*   `fits.Unconstrained model.Rate Distributions.FG`: The distribution of omega rates and their weights inferred for the foreground branches. High weights on omega > 1 indicate positive selection.

## Example Data
- **Expected Output**: `data/example.json`
