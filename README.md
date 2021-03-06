# kubernetes-gpu
Install NVIDIA GPU Support on CoreOS based Kubernetes Cluster

# Prerequisits
* CoreOS based Kubernetes cluster with GPU nodes (e.g. AWS P2 instances)

# Installation
First install the nvidia driver via this daemonset
```
kubectl apply -f https://raw.githubusercontent.com/afritzler/kubernetes-gpu/master/k8s-nvidia-driver.yaml
```
Wait until the init container finishes on each node and install the device plugin
```
kubectl apply -f https://raw.githubusercontent.com/afritzler/kubernetes-gpu/master/k8s-nvidia-deviceplugin.yaml

```


# Run
To run an example training on a GPU node, start first a base image with Tensorflow with GPU support & Keras
```
kubectl apply -f https://raw.githubusercontent.com/afritzler/deeplearning-workbench/master/manifests/dl-workbench.yaml
```
Now `exec` into the container and start an example Keras traing
```
kubectl exec -it deeplearning-workbench-8676458f5d-p4d2v -- /bin/bash
cd /keras/example
python imdb_cnn.py
```

# Open Issues
- [ ] Label GPU nodes and add NodeSelector to daemonset


# Acknowledgments & References
* Build and install NVIDIA driver on CoreOS: https://github.com/squat/modulus
* Nvidia Device Plugin: https://github.com/kubernetes/kubernetes/blob/master/cluster/addons/device-plugins/nvidia-gpu/daemonset.yaml