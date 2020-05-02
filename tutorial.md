# Tutorial

## Prerequisites
1.  Enable APIs
    ```shell script
    gcloud services enable cloudbuild.googleapis.com speech.googleapis.com
    ```
1.  (Optional) Create GCS bucket, if you do not have existing GCS bucket to use
    ```shell script
    export GCS_LOCATION=asia-southeast1
    ```
    ```shell script
    export GCS_BUCKET=gs://bucket
    ```
    ```shell script
    gsutil mb -l $GCS_LOCATION $GCS_BUCKET
    ```
1.  (Optional) Grant current project's Cloud Build service account permission to the GCS bucket 
    (if the GCS bucket resides in different GCP project)
    ```shell script
    export CLOUD_BUILD_SA=$(gcloud projects describe $PROJECT --format 'value(projectNumber)')@cloudbuild.gserviceaccount.com
    gsutil iam ch serviceAccount:$CLOUD_BUILD_SA:roles/storage.objectAdmin $GCS_BUCKET
    ```

## How to Run
1.  Modify the substitution variables in [cloudbuild.yaml](cloudbuild.yaml) based on your needs
    ```yaml
    substitutions:
      # URL where to download audio file from
      _AUDIO_URL: https://eps-dot-gcppodcast.appspot.com/dl/Google.Cloud.Platform.Podcast.Episode.1.mp3
      
      # Path on GCS bucket where to save the files to
      _GCS_FOLDER_PATH: gs://<bucket-name>/gcp-podcast
    
      # The filename that you want to use for the generated files
      _FILENAME: episode1
    ```
1.  Submit a job to Cloud Build
    ```shell script
    gcloud builds submit .
    ```
