<br>

# protein [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics runs only")

---

### `{protein_name}.jp2` & `{protein_name}_thumbnail.png`
> Raw, full-sized protein detection image and its thumbnail. The values in the [cell_metadata](./single_cell_data.md#cell_metadatacsvgz) and [cell_by_protein](./single_cell_data.md#cell_by_proteincsvgz) tables are derived by applying cell segmentation masks over these protein images.

### `bead_mask.npz`
> Compressed numpy archive containing a boolean bead mask. The mask marks regions where focusing beads were detected and is used to remove signal in those areas.