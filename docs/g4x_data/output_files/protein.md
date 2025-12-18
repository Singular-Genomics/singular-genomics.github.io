<br>

# protein
This folder is only present in multiomics runs [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics runs only")

---

### `{protein}.jp2`
> Raw, full-sized protein detection image for `{protein}`. The values in the [cell_metadata](./single_cell_data.md#cell_metadatacsvgz) and [cell_by_protein](./single_cell_data.md#cell_by_proteincsvgz) tables are derived by applying cell segmentation masks over these protein images.

### `{protein}_thumbnail.png`
> Reduced size and compressed thumbnail of each protein image for easy viewing in standard applications. These files are nor suitable for quantitative analysis.

### `bead_mask.npz`
> Compressed numpy archive containing a boolean bead mask. The file marks regions where focusing beads were detected and is used to remove signal in those areas.