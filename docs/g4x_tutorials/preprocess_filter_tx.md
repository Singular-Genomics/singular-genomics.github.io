# Preprocessing Transcript Data

---

<br>

This brief tutorial primarily uses the [scanpy](https://scanpy.scverse.org/en/stable/installation.html) and [numpy](https://numpy.org/install/) libraries and assumes that both are installed and working on your system. If not, please refer to their respective documentation pages for installation instructions. This example uses Python, but equivalent tools are available in most scientific computing languages and can be used to perform similar filtering steps.

<br>

The following preprocessing steps are typically applied to transcript data before downstream analysis:

* remove control probes (NCS, NCP, GCP)
* **filter excessively small and large cells**
* **remove genes present in very few cells**
* **remove cells with poor transcript diversity**
* **remove cells with very low total transcripts**
* count normalize transcript counts per cell
* log transform your data

!!! note "Manual Tuning Required"
For all bolded steps, we recommend adjusting the threshold values based on your dataset. For example, if your data has a median of 250 transcripts per cell, you may choose to remove cells with fewer than 30 transcripts instead of 10. Conversely, if you expect a substantial population of low-transcript cell types, a threshold closer to 10 may be more appropriate. This guide provides general recommendations for each threshold, but incorporating your biological understanding of the system will typically yield better results.

For the steps below, feel free to copy and paste the code into a notebook or download the complete notebook [here](notebook_link). The prebuilt notebook includes additional print statements and more verbose output to help you evaluate and tune your filtering parameters.

<br>

## Preprocessing Steps

---

#### 1. Import libraries

```python
#importing necessary libraries
import numpy as np
import scanpy as sc
import os
```

#### 2. Load your data

```python
# Load your data
adata = sc.read_h5ad('path/to/your/data/single_cell_data/feature_matrix.h5')
```

#### 3. Set parameters for filtering

```python
# Input your desired min and max cell size in um^2.
# This will depend heavily on the expected cell size for your samples.
cell_sz_min = 10
cell_sz_max = 200

# Input your desired min cells for a gene to be included in analysis.
n_cells_min = 50

# Input your desired min unique genes for a cell to be retained.
n_genes_min = 5

# Input your desired min total transcripts for a cell to be retained.
n_txts_min = 10
```

#### 4. Remove control probes

```python
def filter_control_probes(adata):
    """
    Removes all gene targets that are control probes (NCS, NCP, GCP).
    These are not real targets and are mostly used to estimate false 
    discovery rates. Removing them is optional, but they shouldn't be
    used in analysis.
    """
    # control probes to remove
    patterns = ["ncp", "ncs", "gdna", "gcp"]
    
    # Boolean mask: keep genes that do NOT contain any pattern
    keep_genes = [
        not any(p in gene.lower() for p in patterns)
        for gene in adata.var['gene_id']
    ]
    starting_targets = len(adata.var['gene_id'])
    final_targets = sum(keep_genes)
    num_removed=starting_targets-final_targets
    
    print(f"Removed {num_removed} control targets out of {starting_targets} total targets.")
    
    # Subset AnnData (genes = columns)
    return adata[:, keep_genes].copy()

adata = filter_control_probes(adata)
```

#### 5. Remove excessively small and large cells

```python

def filter_cells_by_size(
    adata,
    cell_sz_min=10,
    cell_sz_max=200,
    micron_area_key="nuclei_expanded_area_um",
):
    """
    Filters cells by absolute area, removing any <cell_sz_min or >cell_sz_max

    Parameters
    ----------
    adata : anndata.AnnData
    cell_sz_min : float or None
        Minimum allowed cell area in micron^2 (inclusive).
    cell_sz_max : float or None
        Maximum allowed cell area in micron^2 (inclusive).
    micron_area_key : str
        Column in adata.obs containing areas in um^2.

    Returns
    -------
    anndata.AnnData
        Filtered AnnData object (copy).
    """
    if micron_area_key not in adata.obs.columns:
        raise KeyError(f"'{micron_area_key}' not found in adata.obs.")

    if cell_sz_min is None and cell_sz_max is None:
        raise ValueError("At least one of cell_sz_min or cell_sz_max must be provided.")

    if cell_sz_min is not None and cell_sz_max is not None and cell_sz_min > cell_sz_max:
        raise ValueError("cell_sz_min must be <= cell_sz_max.")

    values = adata.obs[micron_area_key].values

    keep_cells = np.ones(values.shape[0], dtype=bool)
    if cell_sz_min is not None:
        keep_cells &= values >= cell_sz_min
    if cell_sz_max is not None:
        keep_cells &= values <= cell_sz_max

    starting_targets = adata.n_obs
    final_targets = int(keep_cells.sum())
    num_removed = starting_targets - final_targets

    print(f"Removed {num_removed} out of a total {starting_targets} cells.")

    return adata[keep_cells].copy()


adata = filter_cells_by_size(adata, cell_sz_min, cell_sz_max)

```

#### 6. Remove genes found in too few cells

```python
# Remove genes that are detected in fewer than n_cells_min cells
sc.pp.filter_genes(adata, min_cells=n_cells_min)
```

#### 7. Remove cells with too few genes present

```python
# Remove cells with low transcript diversity (< n_genes_min genes)
sc.pp.filter_cells(adata, min_genes=n_genes_min)
```

#### 8. Remove cells with too few total transcripts present

```python
# Remove cells with fewer than n_txts_min total transcript counts
sc.pp.filter_cells(adata, min_counts=n_txts_min)
```

#### 9. Perform count normalization and log transform your matrix

```python
# Normalize counts per cell to a total of 10,000
print(f"\nCount normalizing transcript counts per cell.")
sc.pp.normalize_total(
    adata,
    target_sum=1e4,
    inplace=True
)

# Log-transform the data: log(1 + x)
print(f"\nPerforming a log transform on the cell x gene matrix.")
sc.pp.log1p(adata)
```

#### 10. Save your filtered h5 file

```python
# Save the filtered h5 file
adata.write_h5ad("/path/to/analysis/folder/filtered_feature_matrix.h5")
```

At this point, your data is ready for downstream analysis.

<br>

--8<-- "_core/_partials/end_cap.md"
