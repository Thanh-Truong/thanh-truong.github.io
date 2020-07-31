---
title: 'Setup local Airflow running on Minikube'
date: 2018-11-20
permalink: /posts/2018/11/set-up-local-Airflow-with-Minikube/
tags:
  - airflow
  - minikube
  - kubernetes
  - k8s
published: true
excerpt: ""
---
# Setup local Airflow running on [Minikube](https://kubernetes.io/docs/getting-started-guides/minikube/)

This note assumes OSX as development environment.
## Requirements
- [brew](https://brew.sh/index_sv)
- [GCloud SDK](https://cloud.google.com/sdk/install)
- [Docker for Mac](https://docs.docker.com/docker-for-mac/install/)

## Configuring Minikube
Install Minikube
{% highlight shell %}
brew cask install minikube
{% endhighlight %}

Install Kubectl
{% highlight shell %}
brew install kubernetes-cli
{% endhighlight %}

Start Minikube cluster with `hyperkit` (default VM driver).
{% highlight shell %}
minikube start
{% endhighlight %}


If you are running on a virtual machine, which does not support `hyperkit` natively, please install `virtualbox`

{% highlight shell %}
brew cask install virtualbox
{% endhighlight %}

then start Minikube cluster again with `virtualbox` as driver.

{% highlight shell %}
minikube start --vm-driver=hyperkit
{% endhighlight %}

The `~/.kube/config` contains configurations of clusters so that `kubectl` can interact with. Let use Minkube instead of any other clustering contexts.

{% highlight shell %}
kubectl config use-context minikube
{% endhighlight %}

Review status of Minikube cluster
{% highlight shell %}
minikube status
{% endhighlight %}

Cross validate that `kubectl` is configured to communicate with youre expected `Kubernetes` on Minikube cluster

{% highlight shell %}
kubectl cluster-info
{% endhighlight %}

Open Kubernetes dashboard
{% highlight shell %}
minikube dashboard
{% endhighlight %}

## Preparing Airflow image 

Our customized Airflow image is on Google Container Registry,so that you need an authorized Google account. There are two options

- (*Not recommended !!!!*) Setting up Docker environment on a VM running within Minikube cluster is an option but you need to re-setup everytime after the VM is purged or corrupted.

    {% highlight shell %}
    minikube ssh
    # Setting up Docker environment to build / pull images
    . . .
    {% endhighlight %}

- Let set the Docker environment from the host machine to that Minikube VM  

    {% highlight shell %}
    eval $(minikube docker-env)
    {% endhighlight %}
    Then 
    {% highlight shell %}
    gcloud docker --pull eu.gcr.io/bb-analytics-1/airflow:878afc8
    {% endhighlight %}

    If your docker client is above 18.03, the command gcloud dockeris not supported. Here it is [deprecation-notices](https://cloud.google.com/container-registry/docs/support/deprecation-notices). In that case:
    {% highlight shell %}
    gcloud auth configure-docker
    docker pull eu.gcr.io/bb-analytics-1/airflow:878afc8
    {% endhighlight %}

## Initializing [Helm](https://helm.sh/)

{% highlight shell %}
brew install kubernetes-helm
{% endhighlight %}

Initialize the local CLI and also install Tiller into your Kubernetes cluster.
{% highlight shell %}
helm init
{% endhighlight %}

Let create a service account named `Tiller` and give its permission to create/delete resources on a cluster, namely `cluster-admin`

{% highlight shell %}
kubectl create serviceaccount --namespace kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'      
helm init --service-account tiller --upgrade
{% endhighlight %}

Note that, `Helm` is CLI and `Tiller` is server side of Helm Chart.

## Deploying Airflow on Minikube

- Mounting DAGs folders
    Mounting a DAGs folder into the Minikube
    {% highlight shell %}
    minikube mount ~/Workspace/src/github.com/tv4/data-airflow-dags/dev:/dags
    {% endhighlight %}

    This folder with be forwarded (mounted) into the running pod.

-  Deploying Airflow with Helm
    {% highlight shell %} 
    helm dep update ./charts/airflow
    helm dep build ./charts/airflow
    helm upgrade --install airflow ./charts/airflow --values ./charts/airflow/values-local.yaml
    {% endhighlight %}


## Cleaning
Optionally, force removal of the Docker images created:
{% highlight shell %}
docker rmi eu.gcr.io/bb-analytics-1/airflow:878afc8 -f
{% endhighlight %}

Stop the Minikube VM:
{% highlight shell %}
minikube stop
eval $(minikube docker-env -u)
{% endhighlight %}

Optionally, delete the Minikube VM to save disk space
{% highlight shell %}
minikube delete
{% endhighlight %}

