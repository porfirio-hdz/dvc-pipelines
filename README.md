# dvc-pipelines
 
> This repository serves as a complete solution of the **DVC Pipelines Demo** [repository](https://github.com/porfirio-hdz/dvc-pipelines-demo).
Once you have activated your virtual environment, you can run ```dvc init``` to initialize the repository with dvc (if not done before). And then, you can run ```dvc repro``` to run all pipeline stages.

This repo is intended to instruct how to build a machine-learning pipeline with DVC. The main goal of the repo is to transition from working in a Jupyter notebook to a more modular workflow.

You can create a virtual environment with a tool such as
[virtualenv](https://virtualenv.pypa.io/en/stable/):

```console
$ python -m venv .mlops
$ source .venv/bin/activate
$ pip install -r requirements.txt
```

The step above will ensure we have the correct versions of [DVC](https://dvc.org/) and [DVCLive](https://dvc.org/doc/dvclive).