# nf-core/rtnanopipeline: Output

This document describes the output produced by the pipeline.

## Pipeline overview

**Deafult output directory: `results/`**

Inside output directory, sample results are organized based on barcode/sample directory name. The following files can be found there:

* `classifier_diveristy_taxlevel.csv`
  * Diversity information at every step (read file) of the analysis for the specific classifier tool and taxonomic level.
* `classifier_report_taxlevel.csv`
  * Simple report containing tax name -> read count for the specific classifier tool and taxonomic level. 
* `classifier_report_full.csv`
  * Simple report containing the complete taxonomic information extracted with taxonkit for every read in the dataset. Also available in OTU format.
* `qc_report.csv`
  * Complete QC report file.

## Web Application

All the above output files are also consumed by the web application, providing an interactive visualization mode for results inspection

Get the conda environment for the webapp and start the server:
```bash
cd viz_webapp && python dashboard.py
```
Access the interface with a web browser (http://127.0.0.1:8050/ by default).