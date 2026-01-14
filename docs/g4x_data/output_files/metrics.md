<br>

# metrics

---

### `transcript_core_metrics.csv`
> CSV file containing a set of core metrics for the sample including total transcripts, total area, number of cells and more.

### `protein_core_metrics.csv` [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics runs only")
> CSV file containing core protein metrics for the sample, such as signal-to-noise ratio (SNR), background intensity, and Fisher's exact test scores. Fisher scores assess the co-occurrence of each protein signal with its corresponding transcripts (`<protein>_fisher_score`) and with a random background set (`<protein>_fisher_background_score`). If no protein-gene pairs are present, the value is `unavailable`; if the score cannot be computed, the entry is `NaN`.

### `per_area_metrics.csv`
> CSV file containing a set of metrics for 100&nbsp;µm<sup>2</sup> bins across the sample. Values included area identifiers `x/y_bin`, area coordinates `x/y_bin_min_pixel_coordinate`, and total number of transcripts and number of cells for each area.