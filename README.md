# Hugging Face Video

This repository contains a simple commandline tool for uploading camera trap video data as a dataset to the Hugging Face (HF) platform. HF does not currently support video datasets in their container format (i.e., MP4). This tool converts video data into bytes and stores it in a column-oriented format (i.e., Parquet) to enable many of HF's attractive features (e.g., dataset streaming) for video. It also includes a Python package with IO-optimised transformations to extract frames (in torch.Tensor format) from bytes that are streamed as a dataset from the hub.

## Installation

Install [Anaconda](https://docs.conda.io/en/latest/miniconda.html). Then create a conda environment using the environment file (conda-environment.yml) using the following command.

```bash
conda env create --name envname
pip install -r requirements.txt
```

## Usage

Examples of how to use the commandline tool and the transformations package. Since the tool was developed primarily for inference over video footage, please note that the commandline tool does not yet support the upload of the corresponding labels (where they exist).

### Dataset uploader

```bash
python upload_data.py --path_to_data=videos/ --repo_id=hf_username/my_repo
```

### Transformation

```python
from hf_video import PanAfTransform

# Initialise transform
my_transform = PanAfTransform()

# Transform batched data
for batch in loader: # batches of {"video": bytes, "filename": name}
    t_batch = my_transform(batch)
    output = model(t_batch)
    ...
```