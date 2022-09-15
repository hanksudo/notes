# Google Cloud Platform Notes

- [Cloud Computing Services  |  Google Cloud](https://cloud.google.com/)
- [gcloud  |  Cloud SDK Documentation  |  Google Cloud](https://cloud.google.com/sdk/gcloud/reference)

## Installation

- [Cloud SDK: Command Line Interface  |  Google Cloud](https://cloud.google.com/sdk)

```bash
brew install google-cloud-sdk

# Initial
gcloud init
```

## Config

- [gcloud config  |  Cloud SDK Documentation  |  Google Cloud](https://cloud.google.com/sdk/gcloud/reference/config)

```bash
gcloud config set disable_usage_reporting true
gcloud config list
gcloud config set project $PROJECT_NAME
```

### Region and Zones

- [Regions and Zones  |  Compute Engine Documentation  |  Google Cloud](https://cloud.google.com/compute/docs/regions-zones)

```bash
gcloud config set compute/region asia-northeast1
gcloud config set compute/zone asia-northeast1-a
gcloud config set functions/region asia-northeast1
```

## Projects

```bash
gcloud projects list
gcloud projects create $PROJECT_NAME
gcloud projects delete $PROJECT_NAME
```

## Services

```bash
# List
gcloud services list --available

# Enable
gcloud services enable $SERVICE_NAME
gcloud services enable cloudfunctions
gcloud services enable kgsearch.googleapis.com
```

## Create default application credential

```bash
gcloud auth application-default login
```

## Cluster

```bash
# Get cluster list
gcloud container clusters list

# Get cluster credentials
gcloud container clusters get-credentials $CLUSTER_NAME --zone asia-northeast1-c --project $PROJECT_ID
```

### Node pool

```bash
# Add a node pool
gcloud container node-pools create pool-name --cluster $CLUSTER_NAME

# Create node pool with machine type
# https://cloud.google.com/compute/docs/machine-types
gcloud container node-pools create my-standard-pool \
  --cluster=$CLUSTER_NAME \
  --machine-type=n1-standard-2 \
  --num-nodes=1

# List node pools
gcloud container node-pools list --cluster $CLUSTER_NAME

# Describe node pool
gcloud container node-pools describe my-standard-pool --cluster $CLUSTER_NAME

# Adjust node size
gcloud container clusters resize $CLUSTER_NAME --node-pool my-standard-pool --num-nodes 3
```

## Service Accounts

```bash
# List service accounts
gcloud iam service-accounts list
```

## Disks

```bash
gcloud compute disks list
gcloud compute disks create --size=20GB --zone=asia-northeast1-c $DISK_NAME --type=pd-ssd
gcloud compute disks delete $DISK_NAME
```

## Components

```bash
gcloud components list
gcloud components install kubectl
gcloud components update
```

## Functions

```bash
# deploy
gcloud functions deploy $FUNCTION_NAME --project $PROJECT_ID --runtime go113 --trigger-http

# Logs

gcloud functions logs read --limit 100

# delete
gcloud functions delete $FUNCTION_NAME --region us-central1
```
