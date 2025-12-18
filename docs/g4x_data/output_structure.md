<br>

# Output structure

This page outlines the directory and file structure produced by the G4X processing pipeline. The overview below shows the main folders and files, helping users navigate and understand the organization of the output directory. For detailed descriptions of each file, see the [output files](output_files/index.md) section.

---

```
<root_directory>
│
├── diagnostics
│   └── (intentionally empty)
│
├── g4x_viewer 
│   ├── {sample_id}_multiplex.ome.tiff
│   ├── {sample_id}_nuclear.ome.tiff
│   ├── {sample_id}_HE.ome.tiff
│   ├── {sample_id}_segmentation.bin
│   ├── {sample_id}_transcripts.tar
│   └── {sample_id}_run_metadata.json
│
├── h_and_e 
│   ├── eosin.jp2
│   ├── eosin_thumbnail.png
│   ├── h_and_e.jp2
│   ├── h_and_e_thumbnail.jpg
│   ├── nuclear.jp2
│   └── nuclear_thumbnail.png
│
├── metrics 
│   ├── transcript_core_metrics.csv
|   ├── protein_core_metrics.csv
│   └── per_area_metrics.csv
│
├── protein                             
│   ├── bead_mask.npz
│   ├── {protein_1}.jp2
│   ├── {protein_1}_thumbnail.png
│   ├── {protein_2}.jp2
│   ├── {protein_2}_thumbnail.png
│   └── …
│
├── rna
│   ├── raw_features.parquet
│   └── transcript_table.csv.gz
│
├── segmentation
│   └── segmentation_mask.npz
│
├── single_cell_data
│   ├── feature_matrix.h5         
│   ├── cell_by_protein.csv.gz          
│   ├── cell_by_transcript.csv.gz
│   ├── cell_metadata.csv.gz
│   ├── clustering_umap.csv.gz
│   ├── dgex.csv.gz
│   ├── protein_singlecell_correlation.csv
│   └─── rna_protein_singlecell_correlation.csv
│
├── summary_{sample_id}.html
├── transcript_panel.csv
├── protein_panel.csv                   
├── samplesheet.csv
└── run_meta.json

```
 
 --8<-- "_core/_partials/end_cap.md"
