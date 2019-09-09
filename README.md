This repository contains analysis code for the paper:

[Linking human and artificial neural representations of language.][3] Jon
Gauthier and Roger P. Levy. [2019 Conference on Empirical Methods in Natural
Language Processing][2].

This repository is open-source under the MIT License. If you would like to
reuse our code or otherwise extend our work, please cite our paper:

     @inproceedings{gauthier2019linking,
       title={Linking human and artificial neural representations of language},
       author={Gauthier, Jon and Levy, Roger P.},
       booktitle={TODO},
       year={2019}
     }

## About the codebase

We structure our data analysis pipeline, from model fine-tuning to
representation analysis, using [Nextflow][4]. Our entire data analysis pipeline
is specified in the file [`flow.nf`][TODO].

Visualizations and statistical tests are done in Jupyter notebooks stored in
the [`notebooks`][TODO] directory.

## Running the code

### Hardware requirements

- ~2 TB disk space (for storing brain images, model checkpoints, etc.)
- 8 GB RAM or more
- 1 GPU with > 4 GB RAM (for fine-tuning BERT models)

We strongly suggest running this pipeline on a distributed computing cluster to
save time. The full pipeline completes in several days on an MIT
high-performance computing cluster.

If you don't have a GPU or this much disk space to spare but still wish to run
the pipeline, please ping me and we can make special resource-saving
arrangements.

### Software requirements

There are only two software requirements:

1. [Nextflow][4] is used to manage the data processing pipeline. Installing
   Nextflow is as simple as running the following command:

   ```bash
   wget -qO- https://get.nextflow.io | bash
   ```

   TODO remark about putting this on your PATH
2. [Singularity][5] retrieves and runs the software containers necessary for
   the pipeline. It is likely already available on your computing cluster. If
   not, please see the [Singularity installation instructions][6].

The pipeline is otherwise fully automated, so all other dependencies
(data, BERT, etc.) will be automatically retrieved.

### Starting the pipeline

Simply run the following command in a clean directory.
(Downloading this repository is not necessary.)

```bash
nextflow run hans/nn-decoding -r emnlp2019-final
```

### Configuring the pipeline

For **technical configuration** (e.g. customizing how this pipeline will be deployed on a cluster, see the file [`nextflow.config`][7]). The pipeline is configured by default to run locally, but can be easily farmed out across a computing cluster.
A configuration for the [`SLURM`][8] framework is given in [`nextflow.slurm.config`][8]. If your cluster uses a framework other than SLURM, adapting to it may be as simple as changing a few settings in that file. See the [Nextflow documentation on cluster computing][9] for more information.

For **model configuration** (e.g. customizing hyperparameters), see the header
of the main pipeline in [`main.nf`][10]. Each parameter, written as `params.X`,
can be overwritten with a command line flag of the same name. For example, if
we wanted to run the whole pipeline with BERT models trained for 500 steps
rather than 250 steps, we could simply execute

```bash
nextflow run hans/nn-decoding -r emnlp2019-final \
  --finetune_steps 500
```

### Analysis and visualization

The `notebooks` directory contains Jupyter notebooks for producing the
visualizations and statistical analyses in the paper (and much more):

- `quantitative_dynamic.ipynb` is used to produce the majority of the plots in
  the paper, studying brain decoding across fine-tuning time in different
  models.
- `structural-probes.ipynb` visualizes the structural probe results.
- `predictions.ipynb` produces, among many other things, the RSA analysis on
  model representations.

You can load and run these notebooks by beginning a Jupyter notebook session in
the same directory as you ran the Nextflow pipeline. The notebooks require
Tensorflow and general Python data science tools to function. I recommend using
my `tensorflow` Singularity image as follows:

```bash
singularity run library://jon/default/tensorflow:1.12.0-cpu jupyter lab
```


[1]: https://doi.org/10.1038/s41467-018-03068-4
[2]: TODO
[3]: TODO
