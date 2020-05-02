# audio-transcriber-cloud-build
An example that shows how you can transcribe audio using GCP Cloud Speech-to-Text, running on Cloud Build.

## Pre-requisites
1. Enable APIs
    ```bash
    gcloud services enable cloudbuild.googleapis.com speech.googleapis.com
    ```
1. Create GCS bucket
    ```bash
    gsutil mb -l asia-southeast1 gs://<bucket-name>
    ```

## How to Run
```bash
gcloud builds submit .
```
