<br>

# single_cell_data

---

### `cell_metadata.csv.gz`
> CSV file containing the metadata associated with each cell, including cell_id, cell area, transcript counts and protein content if available. This can be used to build single-cell analysis objects (Seurat/Scanpy) and perform downstream analyses. For more information, see [data import](../data_import.md).


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
> CSV file containing pre-computed cluster annotations and UMAP coordinates for each cell. 

### `dgex.csv.gz`
> CSV file containing differential gene expression (DGEx) results for the pre-computed clusters from the [clustering_umap](#clustering_umapcsvgz) table.


| column  | type | description  |
|-------|-----|----------|
| `gene_id`  | *str* | Gene identifier  |
| `score`  | *float* | Z-score from Wilcoxon rank-sum test  |
| `logfoldchange`  | *float* | LogFoldChange for the given cluster compared to *all* other clusters combined |
| `pval`  | *float* | P-value from Wilcoxon rank-sum test |
| `pval_adj`  | *float* | Adjusted P-value  |
| `pct_nz_group`  | *float* | Percentage of non-zero values in the given cluster.  |
| `pct_nz_reference`  | *float* | Percentage of non-zero values in all cells outside the given cluster.  |
| `cluster_id`  | *str* | Leiden cluster identity  |
| `leiden_res`  | *str* | Leiden clustering resolution that this entry is derived from (1 per gene per cluster) |

### `sc_processed.h5ad`
> AnnData file containing the cell by gene matrix with cell and gene metadata. This file can be loaded into Python and used in `scanpy` or a number of other pipelines. See [data import](../data_import.md). The `.obs` table is equivalent to [cell_metadata](#cell_metadatacsvgz).

### `cell_by_protein.csv.gz` [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics runs only")
> CSV file containing cell x protein intensity matrix. Each entry in the table is the average protein intensity for a given protein in a cell.

### `rna_protein_sc_correlation.csv` [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics runs only")
> CSV file containing a square correlation matrix between selected transcript counts and protein intensity means, computed per cell. Useful for quickly checking concordance between RNA and protein markers (e.g. `CD4` counts vs `CD4_intensity_mean`).

### `protein_sc_correlation.csv` [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics runs only")
> CSV file containing a protein-by-protein correlation matrix of per-cell intensity means, allowing assessment of protein marker co-expression.
