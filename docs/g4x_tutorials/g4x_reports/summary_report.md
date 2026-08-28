<br>

# G4X sample summary report guide

---

The G4X Sample Summary Report provides a high-level overview of the experiment output, data quality, and performance for an individual sample. It brings together run identifiers, decoding and segmentation metrics, exploratory analysis results, and raw images for you to review in a single interactive HTML file.

!!! note "Report contents vary"

    The available sections depend on the assay and on which analysis outputs were generated. For example, protein metrics, protein-correlation plots, and the protein image gallery appear only when protein data are available.

## Opening your sample summary report

---

The report is named `summary_{sample_id}.html` and is located in the root of the [G4X output directory](../../g4x_data/output_structure.md). For example, the report for sample `A02` is named `summary_A02.html`.

You can open the report in either of the following ways:

1. Open the HTML file directly from the output directory. On macOS, Control-click the file and select **Open With > Google Chrome**. On Windows, right-click the file and select **Open with > Google Chrome**. On Linux, open the file with Google Chrome or Chromium.
2. Open the sample in G4X Viewer, select **View G4X Summary Report**, and use **Open in new tab** if you want the report in a separate browser tab. This view may not be available on older data, as it is a feature specific to our Zarr output formats.

!!! tip

    If the report opens in the G4X Viewer, you can review it without navigating away from the current viewer session.

## How to use this report

---

The primary utility of this report is as your first stop to perform basic QC on your G4X data. The report contains several layers of information that may or may not be useful for all samples.

1. Start with **Summary** for the sample identifiers, headline metrics, and spatial distribution of decoded transcripts.
2. Use **Decoding** to review transcript detection, controls, and probe-level quality.
3. Use **Cell Segmentation** to review the segmentation performance and determine if the cell area and transcript distributions match your expectations.
4. Use **Analysis** to explore a rough visualization of spatial and UMAP distributions, clustering results, and differential gene expression generated for the sample. These are not intended as publication-ready analyses, but instead as a tool to determine if a basic analysis captures key spatial features.
5. Use **Image QC** to inspect the nuclear and cytoplasmic images and, when available, the protein image gallery.

!!! note "Is my data good?"

    This guide explains where to find each result and what the displayed values describe. It does not assign universal pass/fail thresholds, as these are panel, block, and sample dependent. The best indicator of data quality is its ability to accurately capture core cell types and tissue structure with low error rates. Median transcripts per cell, though informative, is not sufficient to qualify data is good or bad on its own.

## Report Contents

---

The report header is present on all tabs of the report and identifies the instrument it was run on, the run ID, flow cell number, and sample ID. Use these values to confirm that you are reviewing the intended sample before interpreting the results.

From the landing page of the report, you will be presented with a set of tabs that contain a variety of run information and basic analysis. Use these tabs to jump between the sections decribed below.

- [Summary](#summary)
- [Decoding](#decoding)
- [Cell Segmentation](#cell-segmentation)
- [Analysis](#analysis)
- [Image QC](#image-qc)

<br>

### Summary

---

The **Summary** tab contains the **Key Metrics** and **Region Detail** sections. 

![Summary tab showing the sample identifiers, Key Metrics cards, Region Detail heatmap, and display controls](../../images/g4x_reports/summary_report/summary_tab.png)

#### Key Metrics

The **Key Metrics** section provides a compact sample-level overview listing the following:

- `Number of cells detected:` reports the number of segmented cells in the sample.
- `Median transcripts per cell:` reports the median decoded transcript count across cells.
- `Transcripts per 100 µm²:` normalizes the decoded-transcript count by tissue area. This varies by sample type substantially.
- `Total decoded transcripts:` reports the total number of decoded transcripts in the sample.

#### Region Detail

This section provides an interactable spatial image that overlays transcript density on the tissue image using 100 µm² bins. You can hide or show the heatmap, change the tissue and heatmap opacity, and adjust the maximum value used for the heatmap scale. These controls can make lower-density spatial patterns easier to inspect.

### Decoding

---

The **Decoding** tab summarizes transcript counts and probe-level decoding results. It is useful for reviewing transcript detection, controls, and probe-level quality. Below is an image from a typical report. 

![Decoding tab showing Counts per Gene, Decoding Yield, Negative Controls, and Gene Quality](../../images/g4x_reports/summary_report/decoding_tab.png)

#### Counts per Gene plot

The **Counts per Gene** plot shows each gene by its total decoded-transcript count and transcript-count rank. Mousing over a point on the plot will displays its gene name, rank, and abundance in the sameple. The associated table lists the gene name, transcript count, and rank. Use the gene-name filter to locate an individual target.

#### Decoding Yield

The **Decoding Yield** section reports the total number of decoded transcripts and the number of transcripts per 100 µm².

#### Negative Controls 

The **Negative Controls** section reports false-discovery rates (FDR) for negative control sequences (NCS), negative control probes (NCP), and genomic control probes (GCP). These values provide context for nonspecific or unexpected signal in the decoded data. More details on the control probes and what they are used for can be found on the [G4X Controls](#insert-link-here) page.

!!! note "Expected FDRs"
    In general, NCP and NCS FDR should be below 0.5%, while GCP FDRs shouldbe below 5%. All FDRs are proportional to median transcripts per cell, though, so for very low transcript count samples, these values may be inflated.

#### Gene Quality plot

The **Gene Quality** plot compares the total transcript count for each probe with its mean probe quality score. The plot distinguishes panel probes from negative-control probes and sequences, allowing you to compare count and quality patterns across probe categories.

This plot can be useful to identify which genes were performing above the noise thresholds. Typically, NCS are low quality and low abundance, NCP are medium quality and low abundance, and panel probes are both high quality and high abundance. Because of this, panel probes that are close to the NCS/NCP on this plot might warrant further investigation or be best to ignore for further analysis. Let biological knowledge of your samples guide you, though, as some low abundance transcripts that are only found in rare cell types may deceptively be present among NCS/NCP when plotted on a sample-wide basis.

### Cell Segmentation

---

The **Cell Segmentation** tab summarizes how decoded transcripts were assigned to segmented cells.  This section is primaruly useful for determining segmentation performance and identifying anomalies in your data. Below is an image from a typical report.

![Cell Segmentation tab showing the segmentation metrics and cell-size distribution](../../images/g4x_reports/summary_report/cell_segmentation_metrics.png)

#### Segmentation Metrics

The **Segmentation Metrics** section reports:

- the number of cells detected;
- the percentage of transcripts within cells;
- the median number of unique genes per cell;
- the median number of transcripts per cell; and
- the percentage of empty cells.

#### Segmentation Plots

This section displays three histograms which show the distributions of **Cell Size**, **Genes Per Cell**, and **Transcripts Per Cell**. In general, these plots should display single modality, normally distributed data. Additional modalities or large numbers of low transcript count cells may indicate poor segmentation in the form of oversegmentation on necrotic tissue or fatty tissue regions.

![Cell Segmentation plots showing the genes-per-cell and transcripts-per-cell distributions](../../images/g4x_reports/summary_report/cell_segmentation_distributions.png)

### Analysis

---

The **Analysis** tab contains **Gene Expression Analysis** and, for multiomics runs, **Protein Analysis**. These analyses are not intended as publication-ready analyses, but instead as a tool to determine if a basic analysis captures key spatial features for easy QC of your data.

#### Gene Expression Analysis

**Gene Expression Analysis** describes basic dimensionality reduction and clustering results using the transcript data to enable you to easily determine if key spatial features are being captured in your data.

![Gene Expression Analysis showing transcript-abundance plots, clustering controls and plots, and the differential-expression table](../../images/g4x_reports/summary_report/analysis_gene_tab.png)

The **Transcripts per cell plots** show cells in spatial coordinates and UMAP coordinates, colored by transcript abundance. The report subsets these displays to 5,000 cells for interactive usability.

The **Clustering plots** show the same spatial and UMAP views colored by cluster assignment. This clustering is performed on board using very minimal filtering parameters to remove obvious outliers before performing PCA, Leiden clustering, and UMAP embedding. When multiple clustering resolutions are present, the report allows you to choose which one to display on the plots.

The **Differential Gene Expression by Cluster** section lists genes associated with each cluster in the lowest resolution Lieden clustering result. Use these results to help determine if key spatial clusters are associated with important marker genes. The table can be navigated using the **Next** and **Previous** buttons.

#### Protein Analysis

For multiomics samples, **Protein Analysis** includes a table of protein core metrics and correlation heatmaps for protein-to-protein and protein-to-RNA measurements. 

![Protein Analysis showing protein core metrics and protein correlation heatmaps](../../images/g4x_reports/summary_report/analysis_protein_tab.png)


The **Protein Core Metrics** section describes high-level statistics for each protein included in the protein panel. This includes:

1. `Median Signal`: The median pixel intensity value of the on-tissue protein image.
2. `SNR`: The Ratio of the signal to the background values (estimated from off-tissue signal versus median on-tissue signal).
3. `Fisher Score`: The Fisher exact score for cooccurrence between the protein and their assocaited RNA (not all proteins have assocaited RNA in our gene panels).
4. `Fisher Background Score`: The Fisher exact score for cooccurrence between the protein and 25 random RNA.

The **Protein-Protein Correlation** plot shows the spatial correlation between each of the protein signal channels for on-tissue signals. This is useful to determine specificity of each signal.

The **Protein-RNA Correlation** plot shows the spatial correlation between each of a set of protein signal channels and their associated transcripts. This is useful to determine whether the transcripts and protein signals colocalize as expected in key cell types.


### Image QC

---

The **Image QC** tab provides whole-sample image views for a visual review of tissue coverage and image quality.

![Image QC tab showing the nuclear and cytoplasmic images and the protein panel image gallery](../../images/g4x_reports/summary_report/image_qc_tab.png)

#### Nuclear image

The nuclear and cytoplasmic views provide complementary structural context for the tissue. Inspect the full tissue footprint for clipping, missing regions, debris, uneven intensity, or other spatial artifacts that may warrant follow up investigation.

#### Protein panel image gallery

For multiomics samples, the **Protein Panel Image Gallery** displays an overview image for each protein target and control included in the report. Compare the spatial signal patterns across targets and note channels with unexpected background, saturation, or limited tissue signal.

<br>

--8<-- "_core/_partials/end_cap.md"
