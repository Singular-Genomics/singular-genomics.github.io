<br>

# g4x-viewer.zarr

---

The `g4x-viewer.zarr` directory is a [G4X-viewer](https://g4x-viewer.singulargenomics.com/) compatible Zarr store. It contains optimized representations of images, cell metadata, cell polygons, transcript locations, and summary content for interactive visualization.

These files are intended for visual exploration with the G4X-viewer, not as the primary source for quantitative analysis. For analysis workflows, use the raw images, transcript tables, masks, and `single_cell_data` files documented in this section.

### `images`
> Zarr groups containing multiscale image pyramids for the multiplex image stack and fH&E image. The multiplex image includes nuclear and cytoplasmic stain channels, plus protein channels in multiomics runs.

### `cells`
> Zarr groups containing segmentation-specific cell metadata, polygon vertices, expression matrix indices, protein intensities, UMAP coordinates, and cluster labels. The default segmentation group is `g4x_default_segmentation`.

### `transcripts`
> Tiled Zarr groups containing viewer-ready transcript coordinates, cell assignments, and gene identifiers.

### `misc`
> Additional viewer assets, including `summary.html` when a sample summary report is available.

??? tip "metadata updates"
    The G4X-helpers `viewer` commands can export and import metadata for `images`, `cells`, and `transcripts` in an existing `g4x-viewer.zarr` store.
<br>
