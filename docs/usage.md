# nf-core/rtnanopipeline: Usage

## Table of contents

* [Table of contents](#table-of-contents)
* [Introduction](#introduction)
* [Running the pipeline](#running-the-pipeline)
  * [Updating the pipeline](#updating-the-pipeline)
  * [Reproducibility](#reproducibility)
* [Main arguments](#main-arguments)
  * [`-profile`](#-profile)
  * [`--reads / --reads_rt`](#--reads and --reads_rt)
 
* [Other command line parameters](#other-command-line-parameters)
  * [`--outdir`](#--outdir)
  * [`--email`](#--email)
  * [`--email_on_fail`](#--email_on_fail)
  * [`--max_multiqc_email_size`](#--max_multiqc_email_size)
  * [`-name`](#-name)
  * [`-resume`](#-resume)
  * [`-c`](#-c)
  * [`--custom_config_version`](#--custom_config_version)
  * [`--custom_config_base`](#--custom_config_base)
  * [`--max_memory`](#--max_memory)
  * [`--max_time`](#--max_time)
  * [`--max_cpus`](#--max_cpus)


## Introduction

Nextflow handles job submissions on SLURM or other environments, and supervises running the jobs. Thus the Nextflow process must run until the pipeline is finished. We recommend that you put the process running in the background through `screen` / `tmux` or similar tool. Alternatively you can run nextflow within a cluster job submitted your job scheduler.

It is recommended to limit the Nextflow Java virtual machines memory. We recommend adding the following line to your environment (typically in `~/.bashrc` or `~./bash_profile`):

```bash
NXF_OPTS='-Xms1g -Xmx4g'
```

<!-- TODO nf-core: Document required command line parameters to run the pipeline-->

## Running the pipeline

The typical command for running the pipeline is as follows:

```bash
nextflow run nf-core/rtnanopipeline --reads '*_R{1,2}.fastq.gz' -profile docker
```

This will launch the pipeline with the `docker` configuration profile. See below for more information about profiles.

Note that the pipeline will create the following files in your working directory:

```bash
work            # Directory containing the nextflow working files
results         # Finished results (configurable, see below)
.nextflow_log   # Log file from Nextflow
# Other nextflow hidden files, eg. history of pipeline runs and old logs.
```

## Main arguments

### `-profile`

Use this parameter to choose a configuration profile. Profiles can give configuration presets for different compute environments.

Several generic profiles are bundled with the pipeline which instruct the pipeline to use software packaged using different methods (Docker, Singularity, Conda) - see below.

> We highly recommend the use of Docker, however when this is not possible, Conda is also supported.

The pipeline also dynamically loads configurations from [https://github.com/nf-core/configs](https://github.com/nf-core/configs) when it runs, making multiple config profiles for various institutional clusters available at run time. For more information and to see if your system is available in these configs please see the [nf-core/configs documentation](https://github.com/nf-core/configs#documentation).

Note that multiple profiles can be loaded, for example: `-profile test,docker` - the order of arguments is important!
They are loaded in sequence, so later profiles can overwrite earlier profiles.

If `-profile` is not specified, the pipeline will run locally and expect all software to be installed and available on the `PATH`. This is _not_ recommended.

* `docker`
  * A generic configuration profile to be used with [Docker](http://docker.com/)
  * Pulls software from dockerhub: [`hecrp/nanortax`](http://hub.docker.com/r/nfcore/rtnanopipeline/)
* `conda`
  * Please only use Conda as a last resort i.e. when it's not possible to run the pipeline with Docker or Singularity.
  * A generic configuration profile to be used with [Conda](https://conda.io/docs/)
  * Pulls most software from [Bioconda](https://bioconda.github.io/)
* `test`
  * A profile with a complete configuration for automated testing
  * Includes links to test data so needs no other parameters
* `default`
  * A profile with a complete configuration for automated testing using all classifiers and default parametes
  * Edit this file to quickly set up your own configuration for the classification workflow

<!-- TODO nf-core: Document required command line parameters -->

### `--reads and --reads_rt`

Use this to specify the location of your input FastQ file(s). reads-rt parameter is used for real-time workflows and provide automatic processing of newly generated read files. For example:

```bash
--reads '/seq_path/fastq_pass/**/*.fastq'
--reads_rt '/seq_path/fastq_pass/**/*.fastq'
```

Please note the following requirements:

1. The path must be enclosed in quotes
2. The path may have `*` wildcard characters fpr selecting several directories/read files


If left unspecified, NanoRTax will load the testing dataset (check conf/test.config)

## Database selection command line parameters

Following parameters define the databases used for classification. Make sure to donwload your preferred ones or check README.md for example 16S databases download command.

### `--kraken_db`

Kraken database path.

### `--centrifuge_db`

Centrifuge database path.

### `--blast_db`

BLAST database path.

### `--blast_taxdb`

BLAST taxdb path. This database is important for retrieving original taxa names from BLAST classification outputs.


## Classifier parameters

Parameter description extracted directly from official documentation:

https://www.ncbi.nlm.nih.gov/books/NBK279684/

https://ccb.jhu.edu/software/centrifuge/manual.shtml

### `--cntrf_min_hitlen`

The output directory where the results will be saved.

### `--blast_evalue`

Expect value (E) for saving hits.

### `--blast_max_hsps`

Maximum number of HSPs (alignments) to keep for any single query-subject pair. The HSPs shown will be the best as judged by expect value. This number should be an integer that is one or greater. If this option is not set, BLAST shows all HSPs meeting the expect value criteria. Setting it to one will show only the best HSP for every query-subject pair.


## Other command line parameters

### `--min_read_length --max_read_length`

Read length thresholds for the QC step. Default (1400-1700) selects near full-length 16S rRNA reads for more accurate taxonomic classification

### `--outdir`

The output directory where the results will be saved.

### `-resume`

Add this flag to the command to continue with an stopped run.