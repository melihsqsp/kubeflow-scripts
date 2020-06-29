# Running a training on GCP via TFJob File
Adjust the namespace and image names then apply yaml file via:
```bash
kubectl apply -f parameter-tuning.yaml
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

Other useful commands:
```bash
kubectl get experiment -n kubeflow
kubectl describe experiment -n kubeflow fashion-mnist-parameter-tuning-1 

kubectl get tfjob -n kubeflow
kubectl describe tfjob <metadata-name> -n kubeflow
```