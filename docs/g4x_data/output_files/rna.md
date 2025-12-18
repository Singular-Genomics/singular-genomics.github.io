<br>

# rna

---

### `raw_features.parquet`
> Parquet file containing raw transcript feature information pre-demultiplexing. It lists sequence, coordinates and quality scores of all detected features. This file is required for the [`redemux`](https://docs.singulargenomics.com/G4X-helpers/features/redemux/) feature of G4X-helpers.


| column | dtype | description |
|-------|-----|----------|
| `TXUID` | *str* | Unique identifier for the transcript |
| `x_pixel_coordinate` | *float* | The x-coordinate for the transcript |
| `y_pixel_coordinate` | *float* | The y-coordinate for the transcript |
| `z_layer` | *int* | z layer of identified transcript |
| `confidence_score` | *float* | Mean quality score for the transcript |



### `transcript_table.csv.gz`
> CSV file listing all successfully demultiplexed transcript features. Includes all columns from `raw_features.parquet`, with additional probe names, gene names, and assigned `cell_id` values for each feature. The values in the [cell_by_transcript](./single_cell_data.md#cell_by_transcriptcsvgz) table are derived by applying cell segmentation masks over these transcripts.


| column | type | description |
|-------|-----|----------|
| `x_pixel_coordinate` | *float* | The x coordinate for the transcript|
| `y_pixel_coordinate` | *float* | The y coordinate for the transcript|
| `z_level` | *int* | Z layer index of the transcript |
| `probe_name` | *str* | Probe identifier used for decoding |
| `gene_name` | *str* | Gene symbol for the decoded probe |
| `confidence_score` | *float* | Mean quality score for the transcript |
| `in_nucleus` | *int* | Cell nucleus the transcript is assigned to (0 = not assigned) |
| `cell_id` | *int* | Cell the transcript is assigned to (0 = not assigned) |