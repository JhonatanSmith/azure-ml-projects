# Churn Analysis

In this small project the idea is to do a Churn analysisi within the Azure ML platform. To do so, we will create a first data analysis process (after obtaining the data, of course) and all script and notebooks will be execute in the Azure ML Studio using the Python SDK V2. 

Of course, when working with the Azure ML Studio some requirmentments will be needed. All coding tutorial and explaaition to connect to Azure ML Studio will be explained and detailed. 

# Steps to follow this projects

## Step 1: Create a workspace

To create a workspace, we will use the Azure CLI. This is done in the [notebook 01_create_workspace.ipynb](notebooks/01_create_workspace.ipynb).

## Step 2: Upload data to the workspace

We get data from Kaggle, so we will use the Kaggle Hub API. We store the data in the folder data/churn.csv.

## Step 3: Create a MLTable

In teh same folder than where teh csv was stored, we will create a MLTable. To do so we will use a mltable.yaml file.

```yaml	
type: mltable
paths:
  - file: ./churn.csv

transformations:
  - read_delimited:
      delimiter: ','
      encoding: 'utf-8'
      headers: true
      include_path_column: false
      empty_as_string: true
      skip_blank_lines: true
```

This file will be used to create a MLTable. It tells t Azure ML how to read propperly the file as a MLTable (like a DataFrame in the cloud). 

``NOTE!`` This requires MLTable SDK to be installed. When doing this i was using Python 3.13.1 and apparently the MLTable SDK is not compatible with Python 3.13.1. The alternative solution was to use a simplified version of the mltable.yaml file. 

```yaml
type: mltable
paths:
  - file: churn.csv
```
And with this file, the MLTable was created. 

