<br>

# g4x_viewer

---

The files in this directory are [G4X-viewer](https://g4x-viewer.vercel.app/) compatible representations of the dataset. They contain much of the same information as found in the raw data outputs, but with reduced fidelity to optimize performance for interactive viewing. These files are not intended for quantitative analysis and should be used only for visual exploration with the G4X-viewer. Please refer to the [G4X-viewer documentation](https://docs.singulargenomics.com/G4X-viewer/) for more information.

### `{sample_id}_segmentation.bin`
*Viewer input field:* UPLOAD SEGMENTATION FILE

> Binary file containing cell segmentation polygons and other cell metadata. This file can be updated with new metadata using the [`update_bin`](https://docs.singulargenomics.com/G4X-helpers/features/update_bin/) function of G4X-helpers.

### `{sample_id}_run_metadata.json`
*Viewer input field:* UPLOAD METADATA

> JSON file containing much of the same information as the `run_meta.json` along with core metrics for the specific block (e.g. `total_tissue_area_mm^2`, `median_transcripts_per_cell`). File names match the sample ID (for example: `A01_run_metadata.json`).

### `{sample_id}_transcripts.tar`
*Viewer input field:* UPLOAD TRANSCRIPT FILE

> Tarball containing the viewer-ready transcript locations and metadata (gene names & colors). Load this into the G4X-viewer under "Upload Transcript File".

### `{sample_id}_multiplex.ome.tiff` [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics runs only")
*Viewer input field:* UPLOAD IMAGE FILE

> Multi-channel OME-tiff file containing aggregated images for all protein targets as well as nuclear and cytoplasmic stains.  
__Note:__ On windows, this may appear as `{sample_id}_multiplex.ome`.

### `{sample_id}_HE.ome.tiff`
*Viewer input field:* ADD FH&E IMAGE (Brightfield Settings)

> Single-channel RGB OME-tiff file containing the fH&E image. This file should be loaded in the "Brightfield Images Settings" in the G4X-viewer.   

### `{sample_id}_nuclear.ome.tiff`
*Viewer input field:* UPLOAD IMAGE FILE

> Multi-channel OME-tiff file containing aggregated images for the nuclear and cytoplasmic stains. In multiomics runs, these channels are also included in `{sample_id}_multiplex.ome.tiff`.   

??? tip "ome.tiff compatibility"
    All provided `*.ome.tiff` files can also be loaded into other standard ome.tiff readers, including Fiji, and napari.  
<br>