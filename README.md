# minikeums_poc
POC pour le projet minikeums - Mise en place d'un cluster k8s


# Odoo_NS :
Contains all files to deploy an Odoo project (Odoo apps + PostgeSQL)


# Accès externes aux applications
Pour pouvoir accéder aux applications en dehors du cluster, il faut créér le cluster minikube de cette manière :
- root
- minikube start --driver=none

# Ingress Controller 
With minikube we use the basic ingress controller based on NGINX. 
> minikube addons enable ingress 

# Volumes Management 
1. Use Persistent Volume (PV) / Persistent Volume Claim (PVC): 
We can create a PV that will "fix" the memory we can use. But when a PVC claim for this PV, the memory cannot be modified. 
> For example: A PV with memory = 8Gi ==> all the PVC that claims this PV will have 8Gi as memory capacity.

2. Use StorageClassName (SC) / Persistent Volume Claim (PVC): 
We can create a SC that will attribute the dynamic memory for each PVC. For NFS we can use the following repository : [NFS for StorageClassName](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner)

> helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner

> helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner --set nfs.server=10.251.113.41 --set nfs.path=/srv/pvol

The default class name : nfs-client

> For example: With a SC configured ==> all the PVC that claims this SC could claim the amount a memory he needs.


# Install Namespace
> helm install odoo-test odoo-chart-0.1.0.tgz --namespace odoo-15 --create-namespace

# Package chart (au même niveau que l'index.yaml)
> helm package [directory chart]

# Generate Index yaml (avec le chart packagé) 
> helm repo index [DIR] --merge index.yaml

# Lister les charts du repo Helm
> helm search repo [repo_name]

# Add the helm repo from github 
> helm repo add minikeums --username BDam-Humans --password api_TOKEN_from_github https://raw.githubusercontent.com/Humans-Connexion/minikeums_poc/master/
