# audio-transcriber-cloud-build
Cloud Build config for transcribing audio using GCP Cloud Speech-to-Text.

## Prerequisites
1.  Enable APIs
    ```shell script
    gcloud services enable cloudbuild.googleapis.com speech.googleapis.com
    ```
1.  Create GCS bucket
    ```shell script
    gsutil mb -l asia-southeast1 gs://<bucket-name>
    ```
1.  Grant Cloud Build service account permission to the GCS bucket (if the GCS bucket resides in different GCP project)
    ```shell script
    gsutil iam ch serviceAccount:<project-number>@cloudbuild.gserviceaccount.com:roles/storage.objectAdmin gs://<bucket-name>
    ```

## How to Run
1.  Modify the substitution variables based on your needs
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
