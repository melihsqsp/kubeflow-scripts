# Running a training on GCP via TFJob File

In this example the Docker file based build step is replaced entirely by dynamic pull
of code repo from github which is mounted under `/code` in the host machine. 
Please see `training-gcs.yaml` file.

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
kubectl describe pod <job_name> -n kubeflow
```