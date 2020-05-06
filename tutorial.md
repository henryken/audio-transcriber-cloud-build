# Audio Transcriber Tutorial

This guide will show you how to run the Cloud Build job to transcribe an audio.
Make sure you meet all the prerequisites listed below.

Click the **Start** button to move to the next step.

## Prerequisites
1.  Choose the GCP project where to run the Cloud Build job.
    <walkthrough-project-setup></walkthrough-project-setup>

1.  (Optional) If your current open Cloud Shell project is different than the project you select above.  
    <walkthrough-open-cloud-shell-button></walkthrough-open-cloud-shell-button>  
    Switch to the source code directory.
    ```bash
    cd ~/cloudshell_open/audio-transcriber-cloud-build
    ```

1.  Enable the required APIs.
    <walkthrough-enable-apis apis="cloudbuild.googleapis.com,speech.googleapis.com"></walkthrough-enable-apis>  
    Note: If clicking the above button does not successfully enable the APIs after some time, copy the command below 
    and run in the Cloud Shell. 
    ```bash
    gcloud services enable cloudbuild.googleapis.com speech.googleapis.com
    ```

1.  Choose either option a or b to prepare GCS bucket to use.  
    a.  If you would like to create a new GCS bucket in the current project.
    ```bash
    export GCS_LOCATION=asia-southeast1
    ```
    ```bash
    export GCS_BUCKET=gs://bucket
    ```
    ```bash
    gsutil mb -l $GCS_LOCATION $GCS_BUCKET
    ```
    b. If you would like to use a GCS bucket residing in different GCP project.  
    Grant current project's Cloud Build service account permission to the GCS bucket.
    ```bash
    export GCS_BUCKET=gs://bucket
    ```
    ```bash
    export PROJECT=$(gcloud info --format='value(config.project)')
    export CLOUD_BUILD_SA=$(gcloud projects describe $PROJECT --format 'value(projectNumber)')@cloudbuild.gserviceaccount.com
    gsutil iam ch serviceAccount:$CLOUD_BUILD_SA:roles/storage.objectAdmin $GCS_BUCKET
    ```

## How to Run
1.  Open <walkthrough-editor-select-line filePath="cloudbuild.yaml" startLine="47" startCharacterOffset="0" endLine="47" endCharacterOffset="15">cloudbuild.yaml</walkthrough-editor-select-line> 
    and modify the substitution variables at the bottom of the file. Do not forget to **save** the file.
    ```yaml
    substitutions:
      # URL where to download audio file from
      _AUDIO_URL: <audio-url>
      
      # Path on GCS bucket where to save the files to
      _GCS_FOLDER_PATH: <gcs-folder-path>
    
      # The filename that you want to use for the generated files
      _FILENAME: <filename>
    ```

1.  Submit the job to Cloud Build.
    ```bash
    gcloud builds submit .
    ```

1.  Wait until the job finishes. It may take a while, depending on the duration of your audio file.

1.  Find your transcript txt file in the GCS bucket folder.  
    Modify the env variables based on what you specified in `cloudbuild.yaml`. 
    ```bash
    gsutil cat $_GCS_FOLDER_PATH/$_FILENAME.txt
    ```


## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You have successfully transcribed an audio file using Cloud Speech-to-Text and Cloud Build!
Please let me know how it goes and share with me your experience!
