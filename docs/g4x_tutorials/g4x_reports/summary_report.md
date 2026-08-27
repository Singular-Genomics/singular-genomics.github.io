<br>

# G4X Sample Summary Report Guide

---

The G4X sample summary report provides a high-level overview of the experiment output, data quality, and performance for an individual sample. It brings together run identifiers, decoding and segmentation metrics, exploratory analysis results, and image-quality views in a single interactive HTML file.

!!! note "Report contents vary"

    The available sections depend on the assay and on which analysis outputs were generated. For example, protein metrics, protein-correlation plots, and the protein image gallery appear only when protein data are available.

## Opening your G4X sample summary report

The report is named `summary_{sample_id}.html` and is located in the root of the [G4X output directory](../../g4x_data/output_structure.md). For example, the report for sample `A02` is named `summary_A02.html`.

You can open the report in either of the following ways:

1. Open the HTML file directly from the output directory. On macOS, Control-click the file and select **Open With > Google Chrome**. On Windows, right-click the file and select **Open with > Google Chrome**. On Linux, open the file with Google Chrome or Chromium.
2. Open the sample in G4X Viewer, select **View G4X Summary Report**, and use **Open in new tab** if you want the report in a separate browser tab.

!!! tip

    If the report opens in G4X Viewer, you can review it without navigating away from the current viewer session.

## How to use this report

---

The report supports several levels of review:

1. Start with **Summary** for the sample identifiers, headline metrics, and spatial distribution of decoded transcripts.
2. Use **Decoding** and **Cell Segmentation** to review transcript detection, controls, probe-level quality, cell counts, and cell-level distributions.
3. Use **Analysis** to explore the spatial and UMAP distributions, clustering results, and differential gene expression generated for the sample.
4. Use **Image QC** to inspect the nuclear and cytoplasmic images and, when available, the protein image gallery.

!!! note "Interpreting quality-control metrics"

    This guide explains where to find each result and what the displayed values describe. It does not assign universal pass/fail thresholds. Interpret the metrics in the context of the sample type, tissue area, panel, and run-specific guidance.

## Report contents

---

The report header identifies the instrument, run ID, flow cell, and sample ID. Use these values to confirm that you are reviewing the intended sample before interpreting the results.

### Summary

The **Summary** tab contains the **Key Metrics** and **Region Detail** sections.

![Summary tab showing the sample identifiers, Key Metrics cards, Region Detail heatmap, and display controls](../../images/g4x_reports/summary_report/summary_tab.png)

#### Key Metrics

The **Key Metrics** section provides a compact sample-level overview:

- **Number of cells detected** reports the number of segmented cells in the sample.
- **Median transcripts per cell** reports the median decoded-transcript count across cells.
- **Transcripts per 100 µm²** normalizes the decoded-transcript count by tissue area.
- **Total decoded transcripts** reports the total number of decoded transcript molecules in the sample.

#### Region Detail

The **Region Detail** section overlays transcript density on the tissue image using 100 µm² bins. You can hide or show the heatmap, change the tissue and heatmap opacity, and adjust the maximum value used for the heatmap scale. These controls can make lower-density spatial patterns easier to inspect.

### Decoding

The **Decoding** tab summarizes transcript counts and probe-level decoding results.

![Decoding tab showing Counts per Gene, Decoding Yield, Negative Controls, and Gene Quality](../../images/g4x_reports/summary_report/decoding_tab.png)

#### Counts per Gene

The **Counts per Gene** section plots each gene by its total decoded-transcript count and transcript-count rank. The associated table lists the gene name, transcript count, and rank. Use the gene-name filter to locate an individual target.

#### Decoding Yield

The **Decoding Yield** section reports the total number of decoded transcripts and the number of transcripts per 100 µm².

#### Negative Controls

The **Negative Controls** section reports false-discovery rates for negative-control sequences, negative-control probes, and genomic-control probes. These values provide context for nonspecific or unexpected signal in the decoded data.

#### Gene Quality

The **Gene Quality** plot compares the total transcript count for each probe with its mean probe quality score. The plot distinguishes panel probes from negative-control probes and sequences, allowing you to compare count and quality patterns across probe categories.

### Cell Segmentation

The **Cell Segmentation** tab summarizes how decoded transcripts were assigned to segmented cells.

![Cell Segmentation tab showing the segmentation metrics and cell-size distribution](../../images/g4x_reports/summary_report/cell_segmentation_metrics.png)

The **Segmentation Metrics** section reports:

- the number of cells detected;
- the percentage of transcripts within cells;
- the median number of unique genes per cell;
- the median number of transcripts per cell; and
- the percentage of empty cells.

Three histograms show the distributions of **Cell Size**, **Genes Per Cell**, and **Transcripts Per Cell**. Review the distributions as well as the median values: a median alone does not show the spread, tails, or multiple populations within the sample.

![Cell Segmentation plots showing the genes-per-cell and transcripts-per-cell distributions](../../images/g4x_reports/summary_report/cell_segmentation_distributions.png)

### Analysis

The **Analysis** tab contains **Gene Expression Analysis** and, when protein results are available, **Protein Analysis**.

#### Gene Expression Analysis

The **Transcripts per cell plots** show cells in spatial coordinates and UMAP coordinates, colored by transcript abundance. The report subsets these displays to 5,000 cells for interactive usability.

The **Clustering plots** show the same spatial and UMAP views colored by cluster assignment. When multiple clustering resolutions are present, use the clustering-resolution selector to switch between them.

The **Differential Gene Expression by Cluster** section lists genes associated with each cluster. Use these results to help characterize clusters, then validate the interpretation using the spatial distribution and relevant biological context.

![Gene Expression Analysis showing transcript-abundance plots, clustering controls and plots, and the differential-expression table](../../images/g4x_reports/summary_report/analysis_gene_tab.png)

#### Protein Analysis

For multiomics samples, **Protein Analysis** can include a table of protein core metrics and correlation heatmaps for protein-to-protein and protein-to-RNA measurements.

![Protein Analysis showing protein core metrics and protein correlation heatmaps](../../images/g4x_reports/summary_report/analysis_protein_tab.png)

### Image QC

The **Image QC** tab provides whole-sample image views for a visual review of tissue coverage and image quality.

![Image QC tab showing the nuclear and cytoplasmic images and the protein panel image gallery](../../images/g4x_reports/summary_report/image_qc_tab.png)

#### Nuclear image

The nuclear and cytoplasmic views provide complementary structural context for the tissue. Inspect the full tissue footprint for clipping, missing regions, debris, uneven intensity, or other spatial artifacts that may warrant closer review in G4X Viewer.

#### Protein panel image gallery

For multiomics samples, the **Protein Panel Image Gallery** displays an overview image for each protein target and control included in the report. Compare the spatial signal patterns across targets and note channels with unexpected background, saturation, or limited tissue signal.

<br>

--8<-- "_core/_partials/end_cap.md"
