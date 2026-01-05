## JSON Field Descriptions
*   `BUSTED-PH.Uncorrected P-value.FG`: P-value testing if there is positive selection (omega > 1) specifically on the foreground branches.
*   `BUSTED-PH.Uncorrected P-value.BG`: P-value testing if there is positive selection on the background branches.
*   `BUSTED-PH.Uncorrected P-value.Comparative`: P-value testing if the selective regimes (omega distributions) are statistically different between foreground and background.
*   `fits.Unconstrained model.Rate Distributions.Test`: The inferred distribution of omega rates and their weights for the foreground branches. High weights on omega > 1 indicate positive selection.
*   `fits.Unconstrained model.Rate Distributions.Background`: The inferred distribution of omega rates for the background branches.
*   `BUSTED-PH.Corrected P-value`: Object containing P-values corrected for multiple testing (typically using the Holm-Bonferroni method) for the three tests (FG, BG, Comparative).
*   `BUSTED-PH.Summary`: A natural language interpretation of the results based on the corrected P-values.
*   `Background selection test results`: Detailed results for the test of selection on background branches (LRT statistic and p-value).
*   `Comparative selection test results`: Detailed results for the test of difference between foreground and background rate distributions.
*   `fits.Constrained model`: Parameter estimates for the global null model where positive selection is disallowed (omega rates constrained to ≤ 1).
*   `fits.Same distributions model`: Parameter estimates for the null model where foreground and background branches are forced to share the same rate distribution.
