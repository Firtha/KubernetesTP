# Kubernetes TP ESGI

This repository is a support for trying and testing Kubernetes inside a vagrant VM.
Some commands and tips are given but according to the version of kubectl you use you will surely need to check the official documentation instead.

## Quickstart

- Clone the project

- Build VM with vagrant :

```bash
~/currentProject$ cd cloned_project/
~/currentProject$ vagrant up
```

- Use the VM :

```bash
~/currentProject$ vagrant ssh
```

- Stop / Remove the VM :

```bash
~/currentProject$ vagrant halt
~/currentProject$ vagrant destroy
```

NB: Check/Clone <https://github.com/k8s-school/examples.git> for yaml files examples.

## Commands to input manually after vagrant build

- Install and configure Oh My Zsh (nicer prompt associated to git)

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" -y
```

- Start minikube

```bash
minikube start
```

- Check validity of kubectl installation

```bash
kubectl version [ --short | --client | --server ]
```

Install Kubernetes with Minikube :
<https://kubernetes.io/docs/setup/learning-environment/minikube/>

- Get some useful yaml files as examples

```bash
git clone https://github.com/k8s-school/examples.git
cd examples/
git checkout v1.16
```

## How to configure kubectl

- kubeconfig file: ~/.kube/config

- export KUBECONFIG="/home/vagrant/aws-kubeconfig"

- kubectl --kubeconfig=/home/vagrant/.kube/config

## kubectl commands

List all resources / shortnames

- kubectl api-resources

Describe a resource and get useful details on it

- kubectl explain pod/namespace/deployment/ etc...

Create a namespace

- kubectl create namespace demo

- kubectl --namespace=demo get pods

List namespaces / pods (long and short names)

- [long-version] : kubectl get pods/nodes/namespaces/deploy/all -o wide --show-labels

- [short-version] : kubectl get po/no/ns -o wide

Deploy nginx in the demo namespace

- kubectl --namespace=demo run nginx --image=nginx --restart=Never

- kubectl --namespace=demo get pods

Display/Build a yaml file based on an imperative run command

- kubectl run kuard --image=gcr.io/kuar-demo/kuard-amd64:1 --restart=Never --dry-run -o yaml

- kubectl run kuard --image=gcr.io/kuar-demo/kuard-amd64:1 --requests='cpu=500m,memory=150Mi' --limits='cpu=1000m,memory=300Mi' --port=8080 --restart=Never --dry-run=client -o yaml > custom-kuard.yaml

Launch/destroy a pod from a yaml file

- kubectl apply -f {pod_file.yaml}

- kubectl delete -f {pod_file.yaml}

Send a command to a docker container (similar to docker exec)

- kubectl exec -it {pod_name} {bash command}

Create a deployment with an imperative command

- kubectl create deployment kuard --image=gcr.io/kuar-demo/kuard-amd64:1 --dry-run=client -o yaml

Add a label to a namespace/pod (ex: env=prod to demo)

- kubectl label namespaces/pod demo env=prod

Remove a label from a namespace (same ex)
The - will reset all values of env=

- kubectl label namespaces demo env-

Get pods with label selectors

- kubectl get po -l key1=value1

Put an annotation to a pod

- kubectl annotate pod pod_name key1="value1"

Display annotations

- kubectl get po -o yaml | grep -A1 annotations

Launch a Job

- kubectl run busybox --image=busybox --restart=OnFailure

Create a ClusterIP Service Type

- kubectl expose deploy hello-world --type=ClusterIP --port=80 --target-port=80 --dry-run=client -o yaml  > hello-world-service.yaml

- kubectl apply -f hello-world-service.yaml

Debug a box (launch + opening prompt)

- kubectl run busybox --image=busybox --restart=Never sleep 3600

- kubectl exec -it busybox -- ash

Create an imperative nginx deployment

- kubectl create deployment nginx --image=nginx:1.7.12

Update the replicas count (=3) of a deployment (nginx)

- kubectl scale deploy nginx --replicas=3

To update the scale of replicas you can also edit the deployment
Editing running deployment :

- kubectl edit deploy nginx

Creating new yaml file then edit it

- kubectl create deployment nginx --image=nginx:1.7.12 --dry-run=client -o yaml > nginx-deploy.yaml
- >> Edit the number of replicas in the file nginx-deploy.yaml
