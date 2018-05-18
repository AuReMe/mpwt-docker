# Pathway-Tools Multiprocessing Dockerfile

A dockerfile for multiprocessing Pathway-Tools's PathoLogic inference to create PGDB using batch mode.

### Table of Contents
1. [Requirements](#Requirements)
2. [Data structure](#Data-structure)
3. [Installation and usage](#Installation-and-usage)


Use a [python script](https://gitlab.inria.fr/abelcour/mpwt) with the multiprocessing library to run Pathway-Tools on multiple genome.

## Requirements

Need a Pathway-Tools installer. You can obtained it [here](http://bioinformatics.ai.sri.com/ptools/). The dockerfile is configured with Pathway-Tools 22.0.

## Data structure

Using genbank file the script will create Pathway-Tools input data, then run Pathway-Tools's PathoLogic on them and it will create an output directory containing the result of the analysis.
The script takes a folder name as argument. The folder structure expected is:

    Folder_input
    ├── Folder_for_species_1
    │   └── Genbank_species_1
    ├── Folder_for_species_2
    │   └── Genbank_species_2
    ├── Folder_for_species_3
    │   └── Genbank_species_3
    │

## Installation and usage

To create the image and the container:

```
sudo systemctl start docker

sudo docker build -t my_image .

sudo docker run -ti -v /my/path/to/my/data:/shared --name="my_container" my_image bash
```

Note: the installer must be placed in the same folder than the dockerfile. The name of the file is currently hardcode (so if you use another version it must be changed). It could be fine if the filename is detected (Pathway-Tools installer filename have a specific structure).

Folder_input must be in /my/path/to/my/data.

Inside the container, run:

```
mpwt -f Folder_input
```

The output will be inside each Folder_for_species_1 folder (look at [Data structure](#Data-structure)).

There will be four files with the genbank (genetic-elements.dat, organism-params.dat, pathologic.log, script.lisp). These files are used for creating PGDB, exporting result files and to log the analysis.

A folder is also present, named output. In this folder there is all the result files of the analysis.

The docker has been tested with Pathway-Tools 20.5 and 22.0. The size of the image is araound 5.57Gb.
