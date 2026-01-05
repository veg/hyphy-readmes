# [Analysis Name]

## Overview
Briefly describe the purpose of this analysis. What biological question does it answer? What method does it use?

## Citation
Please cite the following paper(s) if you use this analysis:
- [Reference 1]
- [Reference 2]

## Input Requirements
Describe the expected input data:
- **Sequence Data**: Alignment format, codon usage, etc.
- **Phylogenetic Tree**: Newick string, limitations (e.g., must be rooted/unrooted).
- **Other Parameters**: Specific options or constraints.

## Usage
How to run the analysis using HyPhy.

### Command Line Interface
```bash
hyphy [AnalysisName].bf --alignment [path/to/alignment] --tree [path/to/tree] [other options]
```

### Key Arguments
| Argument | Description | Default |
| :--- | :--- | :--- |
| `--alignment` | Path to the sequence alignment file. | Required |
| `--tree` | Path to the phylogenetic tree file. | Derived from alignment if omitted |
| ... | ... | ... |

## Outputs
Describe the files and data produced by the analysis.

### Standard Output (Console)
Brief description of what is printed to the console during execution.

### JSON Output
Description of the main results file (usually `[alignment].json`).

- **Key Fields**:
    - `...`: ...
    - `...`: ...

### Visualizations
Mention any graphs or plots generated (e.g., SVG, PDF) or how to visualize the JSON (e.g., using HyPhy Vision).

## Interpretation
How should users interpret the results?
- What constitutes a "significant" finding?
- Common pitfalls or caveats.

## LLM-Targeted Information
This section provides context for Large Language Models to assist in interpreting results.

### JSON Field Descriptions
(Optional: Link to a separate schema file if too large)
- `field_name`: Description of what this field represents biologically.

### Analysis Prompt Template
A suggested prompt for users to ask an LLM about these results:
> "I ran the [Analysis Name] on my dataset. Here is the JSON output. Can you explain the main findings regarding [specific biological phenomenon]?"

## Example Data
- **Alignment**: `data/example.fasta`
- **Tree**: `data/example.nwk`
- **Expected Output**: `data/example.json`
