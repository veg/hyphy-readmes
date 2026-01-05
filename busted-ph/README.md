# BUSTED-PH: Gene-wide detection of trait-associated positive selection

## Overview
BUSTED-PH (Branch-Site Unrestricted Statistical Test for Episodic Diversification - Phenotype) is a statistical method designed to identify genes where episodic diversifying selection (EDS) is associated with a binary trait (phenotype). It is particularly useful for detecting signals of convergent evolution where multiple independent lineages (species or clades) share a phenotype.

The method explicitly models and contrasts positive selection across two sets of branches in a phylogenetic tree:

1.  **Foreground (FG)**: Branches associated with the phenotype of interest.
2.  **Background (BG)**: Branches not associated with the phenotype.

BUSTED-PH performs three distinct statistical tests to ensure that the detected selection is specific to the trait:

1.  **Test for Selection on FG**: Is there evidence of positive selection (&omega; > 1) on the foreground branches?
2.  **Test for Selection on BG**: Is there evidence of positive selection on the background branches?
3.  **Test for Difference**: Is the selective regime (distribution of &omega; rates) significantly different between FG and BG branches?

BUSTED-PH acts as a wrapper analysis that passes most user options to the underlying BUSTED method. It executes BUSTED three times to perform the tests above:

-   Once for the **Foreground (FG)** branches.
-   Once for the **Background (BG)** branches.
-   Once using a model where **all branches** share the same selective regime (used as a null model for the "Test for Difference").

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

**Example of a Labeled Tree**:
![Labeled Tree](tree.png)

## Usage
BUSTED-PH is implemented in HyPhy (version 2.5.73 or later).

### Command Line Interface
```bash
hyphy busted-ph --alignment path/to/alignment.fasta --tree path/to/tree.nwk --branches ForegroundLabel [options]
```

### Key Arguments
| Argument | Description | Default |
| :--- | :--- | :--- |
| `--alignment` | Path to the in-frame codon alignment file (supported formats). | Required |
| `--tree` | Path to the phylogenetic tree file (if not included in alignment). | Derived from alignment |
| `--branches` | The label in the tree file denoting the Foreground (FG) branches to test for association. | Required |
| `--rates` | Number of omega rate classes (k) to include in the model (1-10). | 3 |
| `--srv` | Include synonymous rate variation (SRV) in the model (Yes/No). | Yes |
| `--syn-rates` | Number of synonymous rate classes if SRV is enabled (1-10). | 3 |
| `--multiple-hits` | Support for multiple nucleotide substitutions (None, Double, Double+Triple). | None |
| `--error-sink` | Enable a specific rate class to capture alignment errors (Yes/No). | No |
| `--error-sink-bound` | Lower bound for the error class dN/dS (Advanced). | 100 |
| `--error-sink-weight` | Maximum weight allowed for the error class (Advanced). | 0.01 |
| `--grid-size` | Number of points for the initial distributional guess grid search. | 250 |
| `--starting-points` | Number of initial random guesses for optimization. | 1 |
| `--code` | Genetic code to use (Universal, Vertebrate mtDNA, etc.). | Universal |

## Outputs
BUSTED-PH produces a JSON file containing the detailed results of the model fits and hypothesis tests.

### Standard Output
The console will print the progress of the likelihood optimization and a summary of the three likelihood ratio tests (LRTs).

### JSON Output
The primary output is a JSON file (usually named `[alignment].json` or `[alignment].BUSTED.json`).

*   **`BUSTED-PH`**: The high-level summary of the phenotype association tests.

    *   `Corrected P-value`: P-values adjusted for multiple testing (if applicable).
    *   `Uncorrected P-value`: Raw p-values for the three tests (`FG`, `BG`, `Comparative` (Difference)).
    *   `Summary`: A text summary of the conclusion (e.g., "Selection is associated with the phenotype / trait").

*   **`fits`**: Detailed parameter estimates for the different models.

    *   `Unconstrained model`: The full model where FG and BG have independent rate distributions. Look here for the inferred &omega; values (`Rate Distributions`).
    *   `Constrained model`: The null model for the selection tests.
    *   `Same distributions model`: The null model for the difference test (FG and BG share distributions).

## Interpretation
A gene is considered to exhibit **trait-associated episodic diversifying selection (EDS)** if it meets the following criteria:

1.  **Selection on FG**: Significant p-value (<i>p</i> < 0.05) for the FG test.
2.  **No Selection on BG**: Non-significant p-value (<i>p</i> > 0.05) for the BG test.
3.  **Significant Difference**: Significant p-value (<i>p</i> < 0.05) for the Difference (`Comparative`) test.

### Possible Outcomes
Based on the combination of p-values (corrected for multiple testing), BUSTED-PH reports one of the following summaries:

| FG Test | BG Test | Difference Test | Interpretation |
| :--- | :--- | :--- | :--- |
| **Sig** | **Not Sig** | **Sig** | **Selection is associated with the phenotype / trait.** (Primary positive result) |
| **Sig** | **Not Sig** | **Not Sig** | Selection is associated with the phenotype, but there is no significant difference between test and background branches in terms of selective pressure. |
| **Sig** | **Sig** | **Sig** | Selection is acting on the phenotype branches, but is **also** acting on background branches. However, there is a significant difference in selective pressure between them. |
| **Sig** | **Sig** | **Not Sig** | Selection is acting on the phenotype branches, but is **also** acting on background branches. There is no significant difference in selective pressure between them. |
| **Not Sig** | *Any* | *Any* | There is **no evidence** of episodic diversifying selection on test branches; selection is not associated with phenotype/trait. |

**Note**: The "Difference" test is crucial to rule out genes that are under selection across the entire tree (not just the trait lineages).

## LLM-Targeted Information

This section provides context for Large Language Models to assist in interpreting results.

*   **[Analysis Prompt Template](LLM_PROMPT.txt)**: A suggested prompt for users to ask an LLM about these results.
*   **[JSON Field Descriptions](JSON_FIELDS.md)**: Descriptions of the fields in the JSON output.

## Example Data
### Input
- **Alignment**: `data/prestin.nex` (Contains alignment and tree labeled with `{ECHOLOCATORS}`)

### Output Summary (Example)
Running BUSTED-PH on `prestin.nex` with `{ECHOLOCATORS}` as foreground:

*   **Test for Selection on FG**: p = 0.0168 (Significant)
*   **Test for Selection on BG**: p = 0.5000 (Not significant)
*   **Test for Difference**: p < 0.0001 (Significant)
*   **Conclusion**: "Selection is associated with the phenotype / trait"

This indicates that the *Prestin* gene is under positive selection specifically in the echolocating lineages, supporting the hypothesis of convergent evolution for echolocation.
