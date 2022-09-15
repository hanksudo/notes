# Google Cloud Platform Notes

- [Cloud Computing Services  |  Google Cloud](https://cloud.google.com/)
- [gcloud  |  Cloud SDK Documentation  |  Google Cloud](https://cloud.google.com/sdk/gcloud/reference)

## Installation

- [Cloud SDK: Command Line Interface  |  Google Cloud](https://cloud.google.com/sdk)

```bash
brew install google-cloud-sdk
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

## Functions

```bash
# deploy
gcloud functions deploy $FUNCTION_NAME --project $PROJECT_NAME --runtime go113 --trigger-http

# Logs

gcloud functions logs read --limit 100

# delete
gcloud functions delete $FUNCTION_NAME --region us-central1
```
