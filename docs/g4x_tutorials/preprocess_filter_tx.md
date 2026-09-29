# Preprocess and filter transcript data

---

<br>

This tutorial uses [Scanpy](https://scanpy.scverse.org/en/stable/installation.html) and [NumPy](https://numpy.org/install/) to prepare transcript data for downstream analysis. Install both libraries before you begin. The example uses Python, but you can apply the same steps with equivalent tools in other languages.

<br>

The workflow covers these common preprocessing steps:

- Remove control probes (NCS, NCP, GCP, and gDNA)
- **Remove cells outside the expected size range**
- **Remove genes detected in very few cells**
- **Remove cells with low transcript diversity**
- **Remove cells with very low transcript counts**
- Normalize transcript counts per cell
- Log transform the data

!!! note "Tune the filtering to your data"

    Adjust the thresholds used in the four bold steps to fit your data. For example, if the median transcript count is 250 per cell, you might remove cells with fewer than 30 transcripts instead of 10. If you expect many cells with low transcript counts, a threshold closer to 10 may work better. The values below are starting points. Use your knowledge of the sample and review the data distributions before choosing final thresholds.

You can copy the code below into a notebook and adjust it as needed for your analysis.

<br>

## Preprocessing steps

---

#### 1. Import libraries

```python
# Import the required libraries
import numpy as np
import scanpy as sc
```

#### 2. Load your data

```python
# Load the feature matrix
adata = sc.read_h5ad("path/to/your/data/single_cell_data/feature_matrix.h5")
```

#### 3. Set parameters for filtering

```python
# Set the minimum and maximum cell area in um^2.
# Choose values that fit the expected cell sizes in your sample.
cell_sz_min = 10
cell_sz_max = 200

# Set the minimum number of cells in which a gene must be detected.
n_cells_min = 50

# Set the minimum number of unique genes required to keep a cell.
n_genes_min = 5

# Set the minimum total transcript count required to keep a cell.
n_txts_min = 10
```

#### 4. Remove control probes

```python
def filter_control_probes(adata):
    """
    Remove control probes from the feature matrix.

    Control probes help estimate false discovery rates and should not be
    included in downstream analysis.
    """
    # Control probe patterns to remove
    patterns = ["ncp", "ncs", "gdna", "gcp"]

    # Keep genes that do not contain a control probe pattern
    keep_genes = [
        not any(p in gene.lower() for p in patterns)
        for gene in adata.var["gene_id"]
    ]
    starting_targets = len(adata.var["gene_id"])
    final_targets = sum(keep_genes)
    num_removed = starting_targets - final_targets

    print(f"Removed {num_removed} control targets out of {starting_targets} total targets.")

    # Subset AnnData (genes = columns)
    return adata[:, keep_genes].copy()

adata = filter_control_probes(adata)
```

#### 5. Remove cells outside the expected size range

```python
def filter_cells_by_size(
    adata,
    cell_sz_min=10,
    cell_sz_max=200,
    micron_area_key="nuclei_expanded_area_um",
):
    """
    Remove cells with an area outside the selected range.

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
        A copy of the filtered AnnData object.
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

    starting_cells = adata.n_obs
    final_cells = int(keep_cells.sum())
    num_removed = starting_cells - final_cells

    print(f"Removed {num_removed} out of {starting_cells} total cells.")

    return adata[keep_cells].copy()


adata = filter_cells_by_size(adata, cell_sz_min, cell_sz_max)
```

#### 6. Remove genes detected in too few cells

```python
# Remove genes that are detected in fewer than n_cells_min cells
sc.pp.filter_genes(adata, min_cells=n_cells_min)
```

#### 7. Remove cells with too few detected genes

```python
# Remove cells with fewer than n_genes_min detected genes
sc.pp.filter_cells(adata, min_genes=n_genes_min)
```

#### 8. Remove cells with too few transcripts

```python
# Remove cells with fewer than n_txts_min total transcripts
sc.pp.filter_cells(adata, min_counts=n_txts_min)
```

#### 9. Normalize and log transform the matrix

```python
# Normalize counts per cell to a total of 10,000
print("\nNormalizing transcript counts per cell.")
sc.pp.normalize_total(
    adata,
    target_sum=1e4,
    inplace=True
)

# Log-transform the data: log(1 + x)
print("\nLog transforming the cell-by-gene matrix.")
sc.pp.log1p(adata)
```

#### 10. Save the filtered feature matrix

```python
# Save the filtered feature matrix
adata.write_h5ad("/path/to/analysis/folder/filtered_feature_matrix.h5")
```

Your data is now ready for downstream analysis.

!!! note "Review each sample individually"

    Results can vary by tissue, sample, and disease context because of differences in cell composition, background signal, staining quality, and other factors. Review the filtering results for each sample and adjust the thresholds as needed.

<br>

--8<-- "_core/_partials/end_cap.md"
