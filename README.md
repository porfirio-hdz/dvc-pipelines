# dvc-pipelines
 
This repo is intented to instruct how build machine learning pipeline with DVC. The main goal of the repo is to transition from working in a Jupyter notebook to a more modular workflow.

As a first step, it is **recommended** to create a virtual environment with a tool such as
[virtualenv](https://virtualenv.pypa.io/en/stable/):

```console
$ python -m venv .mlops
$ source .venv/bin/activate
$ pip install -r requirements.txt
```

The step above will ensure we have the right versions of [DVC](https://dvc.org/) and [DVCLive](https://dvc.org/doc/dvclive).

### Initialize the repo with DVC init

Run ```dvc init``` to initialize the DVC repository. This command will create several files related to DVC. 

It is recommended to commit the changes to git before continuing.

### Create the first pipeline stage with DVC

The jupyter notebook is divided into different sections or steps which we will use to build each stage of the pipeline. Notice for example, that the first section is the Data Prepare part. So this will be our first pipeline stage.

So we will take this code cell below:

```python
# Load the Penguins dataset
df = pd.read_csv(config['data_load']['dataset_csv'])

# Display the first few rows of the dataset
df.head()
print(df.shape)
# drop NaNs
df = df.dropna(axis=0, how='any')
df = df.drop('Unnamed: 0', axis=1)
```

and transform it into a python script like this:
```python
import argparse
import pandas as pd
from typing import Text
import yaml

from src.utils.logs import get_logger


def data_load(config_path: Text) -> None:
    """Load raw data.
    Args:
        config_path {Text}: path to config
    """

    with open(config_path) as conf_file:
        config = yaml.safe_load(conf_file)

    logger = get_logger('DATA_LOAD', log_level=config['base']['log_level'])

    logger.info('Get dataset')
    # Load the Penguins dataset
    df = pd.read_csv(config['data_load']['dataset_csv'])

    # drop NaNs
    df = df.dropna(axis=0, how='any')
    df = df.drop('Unnamed: 0', axis=1)

    logger.info('Save processed data')
    df.to_csv(config['data_load']['dataset_prepare'], index=False)


if __name__ == '__main__':

    args_parser = argparse.ArgumentParser()
    args_parser.add_argument('--config', dest='config', required=True)
    args = args_parser.parse_args()

    data_load(config_path=args.config)

```