---
layout: post
title: Setup SparkOperator and Minio in K8s [Draft]
redirect_from: "/2020/01/19/setupsparkminiok8s/"
permalink: setup-sparkoperator-minio-k8s
---

This post assumes that you already have a running k8s cluster setup as minikube or in docker-desktop and you are familiar with kubectl commands.

### Install spark operator

#### Create Namespaces
~~~shell
kubectl create ns spark-operator
kubectl create ns spark-jobs
~~~
#### Add Helm repo
~~~shell
helm repo add spark-operator https://googlecloudplatform.github.io/spark-on-k8s-operator
~~~
#### Install Helm chart for Spark 
Webhook must be enabled for mounting volumes to driver and executor nodes and also for environment variables defined on pods to work.
~~~shell
helm install my-spark spark-operator/spark-operator --namespace spark-operator --set sparkJobNamespace=spark-jobs --set webhook.enable=true --set webhook.port=443
~~~

### Install Minio
#### Create Namespaces
~~~shell
kubectl create ns minio
~~~
#### Add Helm repo 
~~~shell
helm repo add minio https://helm.min.io/
~~~
#### Install Helm chart for  Minio
~~~shell
helm install --namespace minio --generate-name minio/minio
~~~

### Setup Minio
Minio will be installed with its access key & secret key which should be used while connecting to the service
There is a separate image for minio cli minio/mc which should be used to upload your data to minio or it can be accessed using UI also for which we should do a port forward of the minio service.

The secrets are base64 encoded so we should decode it before use
 
~~~shell
kubectl get secret -n minio
~~~
Select the secret with type opaque
~~~shell
kubectl get secret -n minio <secret-name> -o json | jq .data.accesskey| sed s/\"//g | base64 -d
~~~
~~~shell
kubectl get secret -n minio <secret-name> -o json | jq .data.secretkey | sed s/\"//g | base64 -d
k get svc -n minio
NAME               	TYPE       	CLUSTER-IP	EXTERNAL-IP	PORT(S)	 AGE
minio-1607076220   	ClusterIP   	10.96.105.119   	<none>       	 9000/TCP   	3d21h
~~~

#### Move data to minio
~~~shell
1. kubectl run -it --rm minio-cli --image=minio/mc --command sh
~~~
Run the following commands inside the container in step 5 and data obtained in step 2,3 & 4
{% highlight shell linenos %}
export MINIO_ACCESS_KEY=4Wlbo3Up5AGWhszdOcs0
export MINIO_SECRET_KEY=4vpeZdUhgpDtxGtYMHbjBhxwxj9Yb1LCUtuq8mRL
export MINIO_HOST=http://<minio-service-name>.<namespace-of-minio-service>.svc.cluster.local:9000
mc config host add myminio ${MINIO_HOST} ${MINIO_ACCESS_KEY} ${MINIO_SECRET_KEY}
mc ls myminio
{% endhighlight %}

Use `mc cp` command to copy data from the container to minio.
	
Use `wget` commands to fetch data to container.
