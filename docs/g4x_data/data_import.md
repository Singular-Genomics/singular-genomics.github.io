<br>

# G4X-data import

The multi-modal output of the G4X spatial sequencer comprises images, tables and annotated data matrices, which allow deep exploration of your sample. The `single_cell_data` folder contains both per-cell counts aggregated from the segmentation and processed analysis outputs. Choose an input based on whether you want to start a new analysis or continue the provided analysis.

| Goal | Input | Contents |
| --- | --- | --- |
| Start a new analysis with your own filtering and normalization | `cell_by_gene.csv.gz` and `cell_metadata.csv.gz` | Raw transcript counts for the segmented cells, including control probes when present, and associated cell metadata. |
| Continue the provided analysis | `sc_processed.h5ad` | Cells and genes retained after filtering, processed RNA values, and analysis results such as PCA, UMAP, and clustering when processing succeeds. |

The processed AnnData can contain fewer cells and genes than the CSV tables. Its `.obs` metadata describe only the retained cells and can include additional analysis annotations.

#### Below we illustrate data import strategies for [Python](#if-you-are-working-in-python) and [R](#if-you-are-working-in-r) users:

<br>

## if you are working in Python
---
Some excellent tools are:

+ `scanpy`: full-featured single-cell analysis suite
+ `rapids-singlecell`: expands scanpy with GPU-support
+ `squidpy`: functionality for the analysis of spatial data
+ `spatial-data`: analysis and management of spatial data (requires data conversion)

<br>

All of these tools accept, or incorporate [`anndata`](https://anndata.readthedocs.io/en/stable/) objects as the basic representation of your single-cell data. The first two methods below load the provided processed AnnData; the third builds a new object from raw counts. These workflows differ in their cell/gene selection, RNA values, and available annotations.

### Understanding the processed AnnData

Processing filters cells using quality measures such as cell area, transcript counts, detected genes, control-probe percentage, and available nuclear stain measurements. Gene filtering retains targeting genes with sufficient cell coverage. The retained counts are then normalized by total counts per cell and transformed with `log1p`.

When preprocessing completes, `adata.X` contains these normalized, log-transformed RNA values. `adata.layers["counts"]` preserves the raw counts for the **same retained cells and genes**. This layer does not restore cells, genes, or control probes removed during filtering; use the CSV count matrix when you need those values. Avoid treating `adata.X` as raw counts or repeating normalization on it unintentionally.

If processing stops before normalization, the saved object may lack the counts layer and some analysis results. Check the available layers and annotations before continuing an analysis.

For multiomics data, `adata.obsm["protein"]` contains the per-cell mean protein intensities, with rows aligned to `adata.obs_names`. The corresponding column names, in order, are stored in `adata.uns["protein_names"]`. Protein intensities are separate from the RNA matrix in `adata.X`:

```py
# After loading the processed AnnData using either method below:
protein_values = adata.obsm["protein"]
protein_names = adata.uns["protein_names"]
```

The CSV import example below loads RNA and cell metadata only. To include protein measurements in a new analysis, also load [`cell_by_protein.csv.gz`](output_files/single_cell_data.md#cell_by_proteincsvgz) and align its rows by `cell_id`.


### 1. G4X-helpers

If you have G4X-helpers v4 installed, you can use the `G4Xsample()` class to access the anndata of your sample via the `load_adata()` method.


```py
import g4x_helpers as g4x

run_base = '/path/to/g4x_data/sample_x1'

sample = g4x.G4Xsample(run_base)
adata = sample.load_adata(remove_nontargeting=False, load_clustering=False) 
```

!!!note "Note: `load_adata()` options"
    
    `load_adata()` provides options that control which data are returned.  
    `processed`: bool (default=True)  
    `remove_nontargeting`: bool (default=False)  
    `load_clustering`: bool (default=False)


### 2. scanpy

You can load the saved processed object directly by pointing scanpy's `read_h5ad()` function to `sc_processed.h5ad` in your G4X-data directory. This preserves the annotations stored in the file; the Helpers example above uses `load_clustering=False` and omits Leiden columns from `.obs`.

```py
from pathlib import Path
import scanpy as sc

run_base = Path('/path/to/g4x_data/sample_x1')
ad_file = run_base / 'single_cell_data' / 'sc_processed.h5ad'

adata = sc.read_h5ad(ad_file)
```

!!!note
    If you have G4X-helpers installed, then `scanpy` will be available as one of its dependencies. If not, please refer to the [scanpy](https://scanpy.readthedocs.io/en/stable/) documentation for installation guides.


### 3. building from raw data

With G4X-helpers, use `G4Xsample.load_adata(processed=False)` to build a new, unfiltered AnnData from the per-cell count and metadata tables and transcript manifest, rather than loading `sc_processed.h5ad`:

```py
import g4x_helpers as g4x

sample = g4x.G4Xsample('/path/to/g4x_data/sample_x1')
adata = sample.load_adata(processed=False)
```

This keeps raw RNA counts in `.X`, including control probes when present, and adds gene annotations and calculated QC metrics. It also includes protein intensities when available. It does not filter cells or genes, normalize or log-transform counts, or compute PCA, UMAP, or clustering. By default, `remove_nontargeting=False` and `load_clustering=False`.

Alternatively, to start a new RNA analysis without G4X-helpers, read the raw counts and cell metadata directly into an `anndata` object. Its `.X` contains raw counts, including control probes when present. The manual example below does not apply filtering or normalization, or load the provided clustering, embeddings, gene metadata, or protein matrix.

```py
from pathlib import Path
import anndata as ad
import pandas as pd
from scipy import sparse

run_base = Path('/path/to/g4x_data/sample_x1')

txcounts_path = run_base / 'single_cell_data' / 'cell_by_gene.csv.gz'
metadata_path = run_base / 'single_cell_data' / 'cell_metadata.csv.gz'

counts = pd.read_csv(txcounts_path, index_col='cell_id')
X = sparse.csr_matrix(counts.values)

metadata = pd.read_csv(metadata_path, index_col='cell_id')

adata = ad.AnnData(X=X, obs=metadata)
adata.var_names = counts.columns
```

!!!note
    If you have G4X-helpers installed, then `anndata`, `pandas` and `scipy` will be available as dependencies.

<br>

## if you are working in R
---

The most feature rich package for single-cell analysis in R is:

+ `Seurat`: full-featured single-cell analysis suite with spatial analysis capabilities

To work with your data in Seurat, it needs to be loaded into a `SeuratObject`, which is an annotated data structure. This example starts from the raw RNA count matrix and cell metadata, so you can apply your own filtering and normalization in Seurat. It does not reproduce the provided processed AnnData analysis or load protein measurements.

```R
library('Seurat')
 
run_base = c('/path/to/g4x_data/sample_x1')

txcounts_path = file.path(run_base, "single_cell_data/cell_by_gene.csv.gz")
metadata_path = file.path(run_base, "single_cell_data/cell_metadata.csv.gz")

counts <- read.csv(txcounts_path, row.names = 1) 
counts <- t(counts) # transpose to match Seurat input
 
metadata <- read.csv(metadata_path, row.names = 1) 
 
sobj <- CreateSeuratObject(counts = counts, meta.data = metadata)
```

--8<-- "_core/_partials/end_cap.md"
