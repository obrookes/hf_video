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
python upload_data.py --path_to_data=videos/ --repo_id=username/dataset
```

Doing this will create a dataset which is retrievable using the Hugging Face `datasets` library:

```python
from datasets import load_dataset

dataset = load_dataset('username/dataset', streaming=True)
```

### HF Video Transformation

Now you can use the `hf_video` package to retrieve videos as PyTorch tensors:

```python
from hf_video import PanAfTransform
from torch.utils.data import DataLoader

# Initialise transform
my_transform = PanAfTransform(
    num_frames=32,
    image_size=224,
    ...
)

# Initialise loader
loader = DataLoader(dataset)

# Transform batched data
for batch in loader: # batches of {"video": bytes, "filename": name}
    t_batch = my_transform(batch) # batches of {"video": torch.Tensor, "filename": name}
    output = model(t_batch)
    ... 
```