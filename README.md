# pyMetaboFlow_NMR
This provides a comprehensive toolkit for NMR/metabolomics data processing and analysis. It includes modules for data extraction from MNova exports, spectral filtering and masking, referencing and alignment, data transformation, normalization, scaling, and advanced multivariate analyses. The processed data can also be exported to MetaboAnalyst.

# NMR/Metabolomics Data Processing Toolkit

This repository contains a comprehensive Python toolkit for processing, analyzing, and visualizing NMR and metabolomics data. It was developed to streamline data workflows in metabolomics research by providing a collection of functions for:

- **Data Extraction & Import:** Reading and converting data exported from MNova (TXT to CSV) and organizing multiple CSV files.
- **Preprocessing:** Filtering chemical shift ranges, masking unwanted spectral regions, and removing unwanted samples.
- **Spectral Referencing & Alignment:** Aligning spectra by referencing a common peak (e.g., DSS at 0.0 ppm) and applying several alignment methods (ICOSHIFT, RAFFT, PAFFT) to correct spectral shifts.
- **Visualization:** Creating interactive plots to display raw, filtered, and aligned spectra, including overlapping plots, vertical multiplots, and histograms.
- **STOCSY Analysis:** Performing Statistical Total Correlation Spectroscopy (STOCSY) to analyze covariance and correlation across the spectra.
- **Data Transformation, Normalization & Scaling:** Applying different data transformations (log, square root, cube root), normalization (Z-score, PQN, etc.), and scaling (mean centering, Pareto, range scaling, etc.) techniques.
- **Multivariate Analysis:** Running Principal Component Analysis (PCA) and Partial Least Squares Discriminant Analysis (PLS-DA) for dimensionality reduction, group separation, and evaluating variable importance (VIP).
- **Export:** Preparing the processed data for further analysis in tools such as MetaboAnalyst.

## Features

- **Modular Design:** The code is organized into clear sections and functions within the `data_processing_NMR.py` module.
- **Interactive Visualizations:** Utilize Plotly and Matplotlib for interactive and static plotting.
- **Advanced Alignment Methods:** Compare multiple spectral alignment approaches to select the most suitable one for your data.
- **Comprehensive Data Workflow:** From raw data extraction to final multivariate analysis and VIP evaluation, the toolkit covers all major steps in metabolomics data processing.
- **Easy Integration:** Designed to be incorporated into Jupyter Notebooks for an interactive and reproducible analysis workflow.

## Installation



## Usage

The repository includes a Jupyter Notebook that walks through a full data processing workflow. Key steps include:

- **Importing Libraries & Module:**  
  Load essential libraries and import the custom `data_processing_NMR` module.

- **Data Import:**  
  Set your working directory and import your NMR data along with metadata.

- **Preprocessing:**  
  Filter the data by chemical shift, mask unwanted regions, and remove irrelevant samples.

- **Spectral Referencing & Alignment:**  
  Reference the spectra to a common peak and align using various methods.

- **Visualization:**  
  Generate plots to inspect raw and processed spectra.

- **STOCSY Analysis:**  
  Perform STOCSY to assess correlations across the spectrum.

- **Data Transformation & Normalization:**  
  Apply transformations, normalization, and scaling.

- **Multivariate Analysis:**  
  Conduct PCA and PLS-DA, evaluate model performance, and plot VIP scores.

- **Export:**  
  Export the processed data for use in MetaboAnalyst.

---

## Optionals
``` python
#### Optional: Align by group
group_class = "ATTRIBUTE_group"
group_specification = "Cecal 4.1"

# Get the filenames corresponding to the specified group.
target_ids = df_metadata.loc[df_metadata[group_class] == group_specification, "ATTRIBUTE_localsampleid"]

# Subset aligned_df by keeping the "Chemical Shift (ppm)" column and only those columns that match the filenames.
df_subset_1 = aligned_df[["Chemical Shift (ppm)"] + list(target_ids)]

aligned_PAFFT_1 = dp.PAFFT_df(df_subset_1, 
                           segSize_ppm= 0.08,  # segment size in ppm
                           reference_idx=0,
                           shift_ppm= 0.05)     # Maximum allowed shift per segment in ppm.


group_specification = "Rectal 4.1"

# Get the filenames corresponding to the specified group.
target_ids = df_metadata.loc[df_metadata[group_class] == group_specification, "ATTRIBUTE_localsampleid"]

# Subset aligned_df by keeping the "Chemical Shift (ppm)" column and only those columns that match the filenames.
df_subset_2 = aligned_df[["Chemical Shift (ppm)"] + list(target_ids)]

aligned_PAFFT_2 = dp.PAFFT_df(df_subset_2, 
                           segSize_ppm= 0.08,  # segment size in ppm
                           reference_idx=0,
                           shift_ppm= 0.05)     # Maximum allowed shift per segment in ppm.

aligned_PAFFT_G_1 = pd.merge(aligned_PAFFT_1, aligned_PAFFT_2, on="Chemical Shift (ppm)", how="outer")

aligned_PAFFT_G = dp.PAFFT_df(aligned_PAFFT_G_1, 
                           segSize_ppm= 0.08,  # segment size in ppm
                           reference_idx=0,
                           shift_ppm= 0.05)     # Maximum allowed shift per segment in ppm.

dp.create_nmr_plot(aligned_PAFFT_G, 
                    x_axis_col='Chemical Shift (ppm)', 
                    start_column=1, 
                    end_column=None, 
                    title='Aligned_PAFFT_NMR Spectra Overlapping',
                    xaxis_title='Chemical Shift (ppm)',
                    yaxis_title='Intensity',
                    legend_title='Samples',
                    output_dir='images', 
                    output_file='Aligned_PAFFT_nmr_spectra_byGroup.html',
                    show_fig=False)

#### Optional: Align by Chemical shift Regions
region_alignments = [
    {
        'region': (-0.5, 6.5),
        'align_func': dp.align_samples_using_icoshift,
        'params': {'n_intervals': 550, 'target': 'maxcorr'}
    },
    {
        'region': (6.5, 10),
        'align_func': dp.PAFFT_df,  # Replace with another function if needed
        'params': {'segSize_ppm': 0.1, 'reference_idx': 0, 'shift_ppm': 0.04}
    }
]

final_aligned_df = dp.apply_alignment_by_regions(aligned_df, region_alignments)

dp.create_nmr_plot(final_aligned_df, 
                    x_axis_col='Chemical Shift (ppm)', 
                    start_column=1, 
                    end_column=None, 
                    title='Aligned_PAFFT_NMR Spectra Overlapping',
                    xaxis_title='Chemical Shift (ppm)',
                    yaxis_title='Intensity',
                    legend_title='Samples',
                    output_dir='images', 
                    output_file='Aligned_PAFFT_nmr_spectra_byCHemShift.html',
                    show_fig=False)

```

## Contributing

Contributions are welcome! If you have suggestions, bug fixes, or improvements, please open an issue or submit a pull request. When contributing, please follow the repository’s coding guidelines and maintain clear documentation.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Contact

For any questions or suggestions, please contact:

- **Ricardo Moreira Borges** – ricardo_mborges@ufrj.br  
- **Stefan Hermann Kuhn** – stefan.kuhn@ut.ee


