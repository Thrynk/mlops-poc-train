# mlops-poc-train

## Introduction

### Context

This repository is intended to reproduce the [MLOps Python repository](https://github.com/microsoft/MLOpsPython/) of Azure.

I wanted to reproduce step by step, the project to understand better the different tools to use, and the overall concept of integrating MLOps in a Machine Learning Project in Microsoft Azure.

### A few words on MLOps Python project

> This reference architecture shows how to implement continuous integration (CI), continuous delivery (CD), and retraining pipeline for an AI application using Azure DevOps and Azure Machine Learning. The solution is built on the scikit-learn diabetes dataset but can be easily adapted for any AI scenario and other popular build systems such as Jenkins or Travis.

You can find the documentation for this reference architecture [here](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/ai/mlops-python).

This architecture respects Azure best practices and help you to have a solid architecture base for your own project.

I will allow myself to add some modifications on the architecture but they will be mentioned further (no worry :wink:)

## Architecture

### Overview

This architecture consists of the following components:

> Azure Pipelines. This build and test system is based on Azure DevOps and used for the build and release pipelines. Azure Pipelines breaks these pipelines into logical steps called tasks. For example, the Azure CLI task makes it easier to work with Azure resources.
>
> Azure Machine Learning is a cloud service for training, scoring, deploying, and managing machine learning models at scale. This architecture uses the Azure Machine Learning Python SDK to create a workspace, compute resources, the machine learning pipeline, and the scoring image. An Azure Machine Learning workspace provides the space in which to experiment, train, and deploy machine learning models.
>
> Azure Machine Learning Compute is a cluster of virtual machines on-demand with automatic scaling and GPU and CPU node options. The training job is executed on this cluster.
>
> Azure Machine Learning pipelines provide reusable machine learning workflows that can be reused across scenarios. Training, model evaluation, model registration, and image creation occur in distinct steps within these pipelines for this use case. The pipeline is published or updated at the end of the build phase and gets triggered on new data arrival.
>
> Azure Blob Storage. Blob containers are used to store the logs from the scoring service. In this case, both the input data and the model prediction are collected. After some transformation, these logs can be used for model retraining.
>
> Azure Container Registry. The scoring Python script is packaged as a Docker image and versioned in the registry.
>
> Azure Container Instances. As part of the release pipeline, the QA and staging environment is mimicked by deploying the scoring webservice image to Container Instances, which provides an easy, serverless way to run a container.
>
> Azure Kubernetes Service. Once the scoring webservice image is thoroughly tested in the QA environment, it is deployed to the production environment on a managed Kubernetes cluster.
>
> Azure Application Insights. This monitoring service is used to detect performance anomalies.

Source : <https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/ai/mlops-python>

### Environment setup

First, we need to create our development and production environments.

#### ***Development environment (Local or on Compute instance in Azure Machine Learning in the development resource group)***

We need to set up a development environment that allows us to share and reproduce experiments.

A good practice is to run your experiments using Jupyter notebooks, but what can we create to allow Data Scientists to share their experiments and reproduce experiments done by other Data Scientists ?

First, we can share our code using git (allows versioning and centralized code for the whole project) and Azure Machine Learning studio to have shared files and experiments.

**Web interface** :

You can either code your experiments in the JupyterLab of a Compute Instance on the ml.azure.com portal.
This Compute instance is linked to your workspace :

- You can code your in the JupyterLab with the Notebooks tab.
- manage :
    1. your datasets.
    2. your experiments.
    3. your pipelines
    4. your models (with versioning).
    5. your deployment points.
    6. your compute instances.
    7. your environments using the web interface.
    8. your datastores.
    9. push to the Git repo with the terminal of your compute instance.

**Visual studio code** :

Or you can use visual studio code if you like to have a version of the code on your laptop and tune your development IDE.
This also allows you to use your own laptop compute power if you prefer to reduce costs for your experiments.

Installation for VS code:

1. Install VS code
2. Install Azure Machine Learning extension
3. Connect to your Azure Account.

You now have the same functionalities than on the web interface in your Visual Studio code environment.

#### ***Production environment***

### Best practices for development

1. Experimenting
    Versioning (Git)
    Sharing in Workspace
    Using versioned datasets
    Tracking experiments using MLFlow
        tracking parameters
        tracking models
        tracking results
    Documenting your experiments
    Environments
2. Go to a 1st version of production-ready code
3. Write pipelines
