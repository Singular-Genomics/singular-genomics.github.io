<br>

# single_cell_data

---

### `cell_metadata.csv.gz`
> CSV file containing the metadata associated with each cell, including cell identifiers, cell area, stain intensities, and transcript QC metrics when calculated. Protein intensities are stored separately in [cell_by_protein.csv.gz](#cell_by_proteincsvgz). This can be used to build single-cell analysis objects (Seurat/Scanpy) and perform downstream analyses. For more information, see [data import](../data_import.md).


| name | type | description |
|------|-----|----------|
| `cell_id` | *str* | Unique cell identifier derived from the segmentation mask |
| `sample_id` | *str* | Sample identifier |
| `tissue_type` | *str* | Tissue type from sample metadata |
| `block` | *str* | Tissue block identifier from sample metadata |
| `seg_source` | *str* | Segmentation source used to generate the cell |
| `cell_x/y` | *float* | x/y coordinate of the cell centroid |
| `cell_area_um` | *float* | Area of the expanded cell segmentation in µm<sup>2</sup> |
| `nuclei_area_um` | *float* | Area of the nuclear segmentation in µm<sup>2</sup> |
| `total_counts` | *int* | Total transcript counts |
| `log1p_total_counts` | *float* | Log number of total transcripts |
| `n_genes_by_counts` | *int* | Number of unique genes detected |
| `log1p_n_genes_by_counts` | *float* | Log number of unique genes detected |
| `total_counts_ctrl` | *int* | Total control probe counts |
| `log1p_total_counts_ctrl` | *float* | Log number of control probe counts |
| `pct_counts_ctrl` | *float* | Percentage of counts assigned to control probes |
| `*stain_intensity_mean` | *float* | Mean intensity for nuclear and cytoplasmic stains |
| `<protein>_intensity_mean` | *float* | Mean intensity for a given protein  [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics runs only")|

### `cell_by_gene.csv.gz`
> CSV file containing the cell x gene matrix. Each entry in the table is the count for a given gene in a given cell. Control probes are included as additional columns when present.

### `clustering_umap.csv.gz`
> CSV file containing pre-computed cluster annotations and UMAP coordinates for cells retained for analysis.

`cell_id`, `UMAP1`, and `UMAP2` are required by the current Helpers validator. Cluster annotation columns depend on the analysis; current Helpers defaults are `leiden_coarse` and `leiden_fine`. They are not universal required column names.

### `dgex.csv.gz`
> CSV file containing differential gene expression (DGEx) results for the pre-computed clusters from the [clustering_umap](#clustering_umapcsvgz) table.


All columns below are required by the current Helpers DGEx schema.

| column  | type | description  |
|-------|-----|----------|
| `gene_id`  | *str* | Gene identifier  |
| `score`  | *float* | Z-score from Wilcoxon rank-sum test  |
| `logfoldchange`  | *float* | LogFoldChange for the given cluster compared to *all* other clusters combined |
| `pval`  | *float* | P-value from Wilcoxon rank-sum test |
| `pval_adj`  | *float* | Adjusted P-value  |
| `pct_nz_group`  | *float* | Fraction of cells with non-zero expression in the given cluster (0–1; e.g. 0.933 means 93.3%).  |
| `pct_nz_reference`  | *float* | Fraction of cells with non-zero expression outside the given cluster (0–1).  |
| `cluster_id`  | *str* | Leiden cluster identity  |
| `leiden_res`  | *str* | Name of the clustering annotation used for this result, such as `leiden_coarse` or `leiden_fine`; this is not a numeric resolution. Each result refers to a gene and cluster within that annotation. |

### `sc_processed.h5ad`
> AnnData file containing filtered and processed RNA data with cell and gene metadata. The retained data is used to call clusters and calculate differential gene expression. Filtering is likely to remove cells and genes present in the raw CSV tables; please refer to [data import](../data_import.md) to learn how to build an unprocessed/raw AnnData object.  
> When preprocessing completes, `.X` contains RNA values normalized by total counts per cell and transformed with `log1p`. `.layers["counts"]` contains raw counts for the same retained cells and genes. PCA, UMAP, and clustering results are included when the corresponding processing steps succeed. Outputs saved after an incomplete processing run may lack some layers or annotations.
> For multiomics data, `.obsm["protein"]` stores mean protein intensities for the retained cells, with column names in `.uns["protein_names"]`. These measurements are separate from the RNA matrix.

### `cell_by_protein.csv.gz` [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics runs only")
> CSV file containing cell x protein intensity matrix. Each entry in the table is the average protein intensity for a given protein in a cell. These columns are separate from the stain intensity columns in `cell_metadata.csv.gz` which is provided for all runs.

### `rna_protein_sc_correlation.csv` [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics runs only")
> CSV file containing a square correlation matrix between selected transcript counts and protein intensity means, computed per cell. Useful for quickly checking concordance between RNA and protein markers (e.g. `CD4` counts vs `CD4_intensity_mean`).

### `protein_sc_correlation.csv` [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics runs only")
> CSV file containing a protein-by-protein correlation matrix of per-cell intensity means, allowing assessment of protein marker co-expression.
