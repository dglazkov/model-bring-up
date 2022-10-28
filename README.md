```
python3 -m venv env
source env/bin/activate
```

Following https://github.com/deepmind/xmanager instructions:

```
pip install -r requirements.txt
<create GCP project>
<download gcloud tool>
gcloud auth login
gcloud auth application-default login
gcloud config set project <insert name of GCP project>
gcloud auth configure-docker

```
Now go to https://github.com/google-research/t5x#quickstart-recommended

```
export GOOGLE_CLOUD_BUCKET_NAME=<insert name of GCP bucket>
export TFDS_DATA_DIR=gs://$GOOGLE_CLOUD_BUCKET_NAME/t5x/data
export MODEL_DIR=gs://$GOOGLE_CLOUD_BUCKET_NAME/t5x/$(date +%Y%m%d)
```

I have no idea what `tfds` is, so I am just going to skip it.

```
git clone https://github.com/google-research/t5x
cd ./t5x/
```

```
python3 ./t5x/scripts/xm_launch.py \
  --gin_file=t5x/examples/t5/t5_1_1/examples/base_wmt_from_scratch.gin \
  --model_dir=$MODEL_DIR \
  --tfds_data_dir=$TFDS_DATA_DIR
```

Error: 

```
HttpError 403 when requesting https://cloudbuild.googleapis.com/v1/projects/model-bring-up-exercises/builds?alt=json returned "Cloud Build API has not been used in project 300987539478 before or it is disabled. Enable it by visiting https://console.developers.google.com/apis/api/cloudbuild.googleapis.com/overview?project=300987539478 then retry`
```

Enabled Cloud Build API. Trying again.

It says `SUCCESS`, but there's another error at the end:

```
HttpError 403 when requesting https://cloudresourcemanager.googleapis.com/v1/projects/model-bring-up-exercises:getIamPolicy?alt=json returned "Cloud Resource Manager API has not been used in project 300987539478 before or it is disabled. Enable it by visiting https://console.developers.google.com/apis/api/cloudresourcemanager.googleapis.com/overview?project=300987539478 then retry
```

Enabled Cloud Resource Manager API. Trying again.

Another error about exceeding quota limits. Giving up for now.