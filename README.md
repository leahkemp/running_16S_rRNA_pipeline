# Running 16S rRNA pipeline

## Table of contents

- [Running 16S rRNA pipeline](#running-16s-rrna-pipeline)
  - [Table of contents](#table-of-contents)
  - [Description](#description)
  - [Setup](#setup)
  - [Run pipeline](#run-pipeline)
  - [Debugging](#debugging)

## Description

Trying to get the [16s-rRNA-Sanger](https://github.com/matrs/16s-rRNA-Sanger) snakemake pipeline working for a colleague

## Setup

Clone pipeline

```bash
cd /home/lkemp/running_16S_rRNA_pipeline/

git clone https://github.com/matrs/16s-rRNA-Sanger.git

cd 16s-rRNA-Sanger
```

Make directories to download databases into

```bash
mkdir databases
```

Get the blast database (see info about the data [here](https://ftp.ncbi.nlm.nih.gov/blast/db/README))

```bash
# Download file into a database 
wget https://ftp.ncbi.nlm.nih.gov/blast/db/16S_ribosomal_RNA.tar.gz

# Unzip/untar file into my "databases" directory
tar -xzf 16S_ribosomal_RNA.tar.gz -C databases
```

Get training data from [RDP Resource Download Area](http://rdp.cme.msu.edu/misc/resources.jsp)

```bash
# Download file
# (note. downloading this file errored out a few times. Re-running the command a few times eventually got it to download)
wget http://rdp.cme.msu.edu/download/rdpclassifiertraindata/data.tgz

# Unzip/untar file into my "databases" directory
tar -xzf data.tgz -C databases
```

Note. it throws this error:

```bash

gzip: stdin: unexpected end of file
tar: Unexpected EOF in archive
tar: Unexpected EOF in archive
tar: Error is not recoverable: exiting now
```

Modify config file `config.yaml`. My config file:

```yaml
# path or URL to sample sheet (TSV format, columns: sample, condition, ...)
samples: samples_short.tsv

# Folder containing the blast db
blast_db: "./databases/data/classifier/16srrna/"

# Folder containing the trainee files for seqmatch
rdp_trainee: "./databases/"

#name of the project
project_name: "PRJNA33175_333"
```

## Run pipeline

Create a screen to run the pipeline in so the pipeline keeps running if the session cuts out

```bash
screen -S rna_sanger_pipeline
```

Create conda environment to run pipeline in (note. we needed a snakemake version greater than 5.7.0, see [this stack overflow question](https://stackoverflow.com/questions/58413114/snakemake-wrappers-fails-to-open-environment-file-http-error-404-not-found))

```bash
conda create -n rna_pipeline_env snakemake=6.3.0
```

Activate conda env

```bash
conda activate rna_pipeline_env
```

Dryrun pipeline

```bash
snakemake --use-conda -n --cores 8
```

Run pipeline

```bash
snakemake --use-conda --cores 8
```

## Debugging

Getting this error:

```bash

```
