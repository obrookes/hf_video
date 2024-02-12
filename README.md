# Huggingface Video

This repository contains a simple commandline tool for uploading camera trap video data as a dataset to the Hugging Face (HF) platform. HF does not currently support video datasets in their container format (i.e., MP4). This tool converts video data into bytes and stores it in a column-oriented format (i.e., Parquet) to enable many of HF's attractive features (e.g., dataset streaming) for video. It also includes a Python package with IO-optimised transformations to extract frames (in torch.Tensor format) from bytes that are streamed from the hub.

## Installation

Install [Anaconda](https://docs.conda.io/en/latest/miniconda.html). Then create a conda environment using the environment file (conda-environment.yml) using the following command.

```bash
conda env create --name envname --file=conda-environment.yml
conda activate envname
```

## Usage

Example use cases from the commandline tool and the transformations package.

### Dataset uploader

```bash
python upload_data.py --path_to_data=videos/ --repo_id=hf_username/my_repo
```

### Transformation

```
from hf_video import PanAfTransform

# Initialise transform
my_transform = PanAfTransform()

# Transform batched data
for batch in loader:
    t_batch = my_transform(batch)
    output = model(t_batch)
    ...
```