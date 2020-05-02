# Audio Transcriber Tutorial

This guide will show you how to run the Cloud Build job to transcribe an audio.
Make sure you meet all the prerequisites listed below.

Click the **Start** button to move to the next step.

## Prerequisites
1.  Choose the GCP project where to run the Cloud Build job
    <walkthrough-project-setup></walkthrough-project-setup>

1.  (Optional) Open Cloud Shell based on the selected project above (if it's different than the one currently open).
    <walkthrough-open-cloud-shell-button></walkthrough-open-cloud-shell-button>

1.  Enable the required APIs
    <walkthrough-enable-apis apis="cloudbuild.googleapis.com,speech.googleapis.com"></walkthrough-enable-apis>
    Copy the command below if the clicking above button does not successfully enable the APIs.
    ```bash
    gcloud services enable cloudbuild.googleapis.com speech.googleapis.com
    ```

1.  (Optional) Create GCS bucket, if you do not have existing GCS bucket to use
    ```bash
    export GCS_LOCATION=asia-southeast1
    ```
    ```bash
    export GCS_BUCKET=gs://bucket
    ```
    ```bash
    gsutil mb -l $GCS_LOCATION $GCS_BUCKET
    ```
1.  (Optional) Grant current project's Cloud Build service account permission to the GCS bucket 
    (if the GCS bucket resides in different GCP project)
    ```bash
    export PROJECT=$(gcloud info --format='value(config.project)')
    export CLOUD_BUILD_SA=$(gcloud projects describe $PROJECT --format 'value(projectNumber)')@cloudbuild.gserviceaccount.com
    gsutil iam ch serviceAccount:$CLOUD_BUILD_SA:roles/storage.objectAdmin $GCS_BUCKET
    ```

## How to Run
1.  Open <walkthrough-editor-select-line filePath="cloudbuild.yaml" startLine="47" startCharacterOffset="0" endLine="47" endCharacterOffset="15">cloudbuild.yaml</walkthrough-editor-select-line> 
    and modify the substitution variables at the bottom of the file
    ```yaml
    substitutions:
      # URL where to download audio file from
      _AUDIO_URL: <audio-url>
      
      # Path on GCS bucket where to save the files to
      _GCS_FOLDER_PATH: <gcs-folder-path>
    
      # The filename that you want to use for the generated files
      _FILENAME: <filename>
    ```
1.  Submit the job to Cloud Build
    ```bash
    gcloud builds submit .
    ```

## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You have successfully transcribed an audio file. Your transcript txt file can be found in the GCS bucket folder you
set in `_GCS_FOLDER_PATH`. 
