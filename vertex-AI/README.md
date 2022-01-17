# Problem Statement
- solve predictive CLV - customer lifetime value(predict future revenue using patterns through which past revenue was collected)
  - limitation-1 : cannot be performed on a new customer.

# Objectives
- Train a TensorFlow model locally in a hosted Vertex Notebook.
- Create a managed Tabular dataset artifact for experiment tracking.
- Containerize your training code with Cloud Build and push it to Google Cloud Artifact Registry.
- Run a Vertex AI custom training job with your custom model container.
- Use Vertex TensorBoard to visualize model performance.
- Deploy your trained model to a Vertex Online Prediction Endpoint for serving predictions.
- Request an online prediction and explanation and see the response.

# Tech Stack
- bigquery and tensorflow
- <font color="red">find why these 2?</font>
  - ease of integration with Vertex-AI
  - successor to AI Platform , <font color="red" size="5">search why??</font>
  - extensive documentation and other resources
  - <font color="red">check whether these are **FASTER**</font>

# Setup

## Enable Cloud Services
- these are the ones you usually use UI for:
`gcloud services enable \
  compute.googleapis.com \
  iam.googleapis.com \
  iamcredentials.googleapis.com \
  monitoring.googleapis.com \
  logging.googleapis.com \
  notebooks.googleapis.com \
  aiplatform.googleapis.com \
  bigquery.googleapis.com \
  artifactregistry.googleapis.com \
  cloudbuild.googleapis.com \
  container.googleapis.com
`
  - google compute, identity access and management, 
  - list, enable and disable APIs and services: `gcloud services list --help`
    -  `gcloud services list --available | grep 'compute'`
- [gcloud reference](https://cloud.google.com/sdk/gcloud/reference?hl=en)

## Create Vertex AI custom service account for Vertex Tensorboard integration
- Create the service account within this project.
  - `SERVICE_ACCOUNT_ID=vertex-custom-training-sa
gcloud iam service-accounts create $SERVICE_ACCOUNT_ID  \
    --description="A custom service account for Vertex custom training with Tensorboard" \
    --display-name="Vertex AI Custom Training"`
  - [doc](https://cloud.google.com/sdk/gcloud/reference/iam/service-accounts/create)
- Grant it access to GCS for writing and retrieving Tensorboard logs
  - `PROJECT_ID=$(gcloud config get-value core/project)
gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member=serviceAccount:$SERVICE_ACCOUNT_ID@$PROJECT_ID.iam.gserviceaccount.com \
    --role="roles/storage.admin"`
    - [`gcloud config get-value`](https://cloud.google.com/sdk/gcloud/reference/config/get-value) . this lets you access property value. <font color="red">find out more Cloud SDK properties</font>. 
- Grant it access to your BigQuery data source to read data into your TensorFlow model
  - `Grant it access to your BigQuery data source to read data into your TensorFlow model`
- Grant it access to Vertex AI for running model training, deployment, and explanation jobs.
  - `gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member=serviceAccount:$SERVICE_ACCOUNT_ID@$PROJECT_ID.iam.gserviceaccount.com \
    --role="roles/aiplatform.user"`

## Create new Vertex Notebook
- Vertex-AI > New Notebook > Tensorflow Enterprise > Tensorflow 2.3 LTS > without GPU > region = us-central1
  - let it run till the status changes from running(rotating arc) to OPEN JUPYTERLAB.

## Clone the lab repo
- `cd ;git clone https://github.com/GoogleCloudPlatform/training-data-analyst`


## Install dependencies
- `cd training-data-analyst/self-paced-labs/vertex-ai/vertex-ai-qwikstart;pip install -U -r requirements.txt`

# Notebook related doubts
- how is CLV even defined?
  -  Can this determination technique have errors?

