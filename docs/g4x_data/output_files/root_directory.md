<br>

# root directory

---

### `summary_{sample_id}.html`
> HTML file which gives a high level overview of the experiment outputs, data quality, and performance for the specific sample.

### `run_meta.json`
> JSON file containing global run information, such as software version, instrument ID, and platform details. It also includes the source files used for the transcript and protein panels.

### `samplesheet.csv` 
> CSV file containing detailed run information, including operator, experimental design, run parameters, flow cell layout, tissue type, and panels used. Also specifies the position of each tissue section on the flow cell.

### `transcript_panel.csv`
> CSV file containing a full list of all probes used to demultiplex the detected features in the data.

### `protein_panel.csv` [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics runs only")
> CSV file containing a full list of all targeted proteins in this experiment and the panel type (standard, addon).