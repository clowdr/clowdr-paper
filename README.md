# Clowdr paper

Repository contents: latex source (`doc/`), and replication materials (`experiment/`).

----

## Paper Replication Instructions

The following instructions have been tested on Mac OSX, but are expected to run equivalently on Linux. For Windows machines, these can either a) be run within the Ubuntu shell (if available), b) a Docker container, or c) a virtual machine.


### Install Clowdr

To install the version of Clowdr run in this paper, ensure that you have Python3 and the corresponding `pip` package installed, and then run:

    pip install clowdr==0.1.0


### Replication 1: Visualizing HCP experiment

Presented in Figure 3 within the manuscript was an analysis of a subset of the HCP dataset as processed with the ndmg pipeline. The summary of this execution can be visualized using the following command:

    clowdr share ./experiment/hcp/clowdr/clowdr-summary.json

Then, navigate in your web browser to [0.0.0.0:8050](0.0.0.0:8050) to view and interact with the figures.

### Replication 2: Processing BIDS data.

To get a sense of the entire Clowdr workflow from scratch, you can also follow the guide below to run your own Clowdr experiment.

#### Downloading data

The dataset used in this paper was a BIDS Example dataset, provided via OpenNeuro. The DS114 dataset can be downloaded using the Amazon web-services command-line tool to the /data/ folder using the following commands:

    pip install awscli
    mkdir -p /data/ds114/
    aws s3 cp --no-sign-request --recursive s3://openneuro/ds000114/ds000114_R2.0.1/uncompressed/ /data/ds114/


#### Running an experiment

Assuming you have Docker or Singularity installed locally, and are running the experiment from the experiment directory, the analysis included in this manuscript can be re-run as shown below. Note: it is expected to have different timing and memory performance when re-running this analysis than was published, as this varies from machine to machine.

    clowdr local \
        bids-example-descriptor.json \
        bids-example-invocation.json \
        ./task/ \
        -bV \
        -g 4 \
        -v /data/ds114:/data/ds114

You can interpret this command as follows: `local` means that the analysis will be run sequentially on the local system. `bids-example-descriptor.json` is the name of the tool description. `bids-example-invocation.json` is the set of invocation properties to be used in analysis. `./task/` is the output directory for Clowdr summary information, equivalent to what is stored in the `bids-example/` directory here. `-bV` controls both the `--bids` flag and `--verbose`, meaning that the invocation will be interpreted as a BIDS app and log outputs will be printed to the terminal, respectively. `-g 4` is the `--group` flag which groups four tasks at a time to be launched by Clowdr. `-v /data/ds114/:/data/ds114/` mounts the `/data/ds114/` directory of the host system to the Docker or Singularity container, at the same location.

The visualiation of results can then be performed with the following line, where the `<clowdr-location>` value is replaced with the full path to the directory created by Clowdr within `./task/` for your experiment.

    clowdr share <clowdr-location>

This will print the URL [http://127.0.0.1:8050/](http://127.0.0.1:8050/) to your terminal, which you can navigate to for exploring your dataset and exporting figures.


#### Reprozip around containerized tools

The issue pertaining to Docker or Singularity and Reprozip execution tracing can be seen discussed in the Github issues on respective projects, [here](https://github.com/singularityware/singularity/issues/1529) and [here](https://github.com/ViDA-NYU/reprozip/issues/294). A simplified summary of these discussions can be found in the manuscript.
