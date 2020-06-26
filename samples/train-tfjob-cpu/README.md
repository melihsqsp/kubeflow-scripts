# Running a training on GCP via TFJob File
Running a training job has 2 parts: First a docker image with the training script
is built locally and uploaded to GCR. Then a TFJob resource is created
on the Kubernetes cluster. The config file points to the image that was uploaded
and other parameters needed such as location to upload the trained model to and 
other arguments. See `training-gcs.yaml` file.

## Building and pushing the docker image: 
Useful variables to set before we start:
``` bash
PROJECT_ID=$(gcloud config get-value core/project)
IMAGE_NAME=kubeflow-fashion-mnist-train-keras-gpu
IMAGE_VERSION=latest
IMAGE_NAME=gcr.io/$PROJECT_ID/$IMAGE_NAME
```

Build the image:
``` bash
docker build -t $IMAGE_NAME:$IMAGE_VERSION .
docker push $IMAGE_NAME:$IMAGE_VERSION
```

## Run TFJob

If not done already create a bucket to store the tensorflow model
```bash
gsutil mb gs://$PROJECT_ID-fashion-mnist-tfjob/
```

Edit the training yaml: Set a `name` under `metadata`. Set the image containers to 
point to the image that was just pushed. Set `tf-export-dir` to the directory 
to save the model to. Then submit the training:

```bash
kubectl apply -f training-gcs.yaml
```

### Monitor:
Logs can be tailed via:
```bash
kubectl logs -f <metadata-name>-chief-0 -n kubeflow
```
pod name can be found with:
```bash
kubectl get pods -n kubeflow | grep job
```

Other useful commands related to tfjob CRD:
```bash
kubectl get tfjob -n kubeflow 
kubectl describe tfjob <job_name> -n kubeflow
```