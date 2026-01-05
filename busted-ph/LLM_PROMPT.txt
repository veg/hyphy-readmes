## Analysis Prompt Template
Use the following prompt to ask an LLM to interpret your BUSTED-PH JSON results:

> "I ran the BUSTED-PH analysis on my dataset to detect positive selection associated with [Trait Name].
>
> Here are the key results from the JSON output:
> - **Test for Selection on Foreground (FG)**: p-value = [Insert `BUSTED-PH.Uncorrected P-value.FG`]
> - **Test for Selection on Background (BG)**: p-value = [Insert `BUSTED-PH.Uncorrected P-value.BG`]
> - **Test for Difference (Comparative)**: p-value = [Insert `BUSTED-PH.Uncorrected P-value.Comparative`]
>
> **Foreground Rate Distribution (from `fits.Unconstrained model.Rate Distributions.Test`)**:
> [Paste the `Test` rate distribution block here]
>
> **Background Rate Distribution (from `fits.Unconstrained model.Rate Distributions.Background`)**:
> [Paste the `Background` rate distribution block here]
>
> Based on these results, is there evidence for trait-associated positive selection? Explain if the selection is specific to the trait or widespread."
