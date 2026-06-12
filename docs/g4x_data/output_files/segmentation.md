<br>

# masks

---

### `segmentation_mask.npz`
> Compressed NumPy archive containing segmentation masks. This archive holds two arrays: `nuclei` (nuclear segmentation) and `nuclei_exp` (expanded nuclear mask, i.e. whole-cell approximation). Label IDs correspond to the `cell_id` fields used in [single_cell_data](./single_cell_data.md).

### `bead_mask.npz`
> Compressed NumPy archive containing a boolean bead mask under the `bead_mask` key. The mask marks regions where focusing beads were detected and is used to remove signal in those areas.
