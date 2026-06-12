<br>

# Output directory structure

This page outlines the directory and file structure produced by the G4X processing pipeline. The overview below shows the main folders and files, helping users navigate and understand the organization of the output directory. For detailed descriptions of each file, see the [output files](output_files/index.md) section.

---
=== "Multiomics Run [<span class="acc-2-text">:material-layers-outline:</span>]("multiomics run")"
    ```
    <root_directory>
    ├── sample.g4x
    ├── samplesheet.csv
    ├── transcript_manifest.csv
    ├── protein_panel.csv                   
    ├── summary_{sample_id}.html
    │
    ├── g4x-viewer.zarr
    │   ├── images
    │   ├── cells
    │   ├── transcripts
    │   └── misc
    │
    ├── h_and_e 
    │   ├── h_and_e.ome.tiff
    │   ├── nuclear.ome.tiff
    │   ├── cytoplasmic.ome.tiff
    │   └── thumbs
    │
    ├── metrics 
    │   ├── transcript_core_metrics.csv
    │   ├── protein_core_metrics.csv
    │   └── per_area_metrics.csv
    │
    ├── protein                             
    │   ├── {protein_1}.ome.tiff
    │   ├── {protein_2}.ome.tiff
    │   ├── thumbs
    │   └── …
    │
    ├── rna
    │   ├── raw_features.parquet
    │   └── transcript_table.csv.gz
    │
    ├── masks
    │   ├── segmentation_mask.npz
    │   └── bead_mask.npz
    │
    ├── single_cell_data
    │   ├── sc_processed.h5ad         
    │   ├── cell_by_gene.csv.gz
    │   ├── cell_by_protein.csv.gz          
    │   ├── cell_metadata.csv.gz
    │   ├── clustering_umap.csv.gz
    │   ├── dgex.csv.gz
    │   ├── protein_sc_correlation.csv
    │   └── rna_protein_sc_correlation.csv

    ```
 
=== "Transcript Run"
    ```
    <root_directory>
    ├── sample.g4x
    ├── samplesheet.csv
    ├── transcript_manifest.csv
    ├── summary_{sample_id}.html
    │
    ├── g4x-viewer.zarr
    │   ├── images
    │   ├── cells
    │   ├── transcripts
    │   └── misc
    │
    ├── h_and_e 
    │   ├── h_and_e.ome.tiff
    │   ├── nuclear.ome.tiff
    │   ├── cytoplasmic.ome.tiff
    │   └── thumbs
    │
    ├── metrics 
    │   ├── transcript_core_metrics.csv
    │   └── per_area_metrics.csv
    │
    ├── rna
    │   ├── raw_features.parquet
    │   └── transcript_table.csv.gz
    │
    ├── masks
    │   ├── segmentation_mask.npz
    │   └── bead_mask.npz
    │
    ├── single_cell_data
    │   ├── sc_processed.h5ad               
    │   ├── cell_by_gene.csv.gz
    │   ├── cell_metadata.csv.gz
    │   ├── clustering_umap.csv.gz
    │   └── dgex.csv.gz

    ```

 --8<-- "_core/_partials/end_cap.md"
