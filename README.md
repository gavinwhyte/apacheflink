# apacheflink

Kubernetes Deployment

Simple Kubernetes Flink cluster resources

As part of the standard Kubernetes Deployment for Flink listed below

A basic Flink cluster deployment in Kubernetes has three components:

a Deployment for a single Jobmanager
a Deployment for a pool of Taskmanagers
a Service exposing the Jobmanager?s RPC and UI ports

kubectl create -f jobmanager-deployment.yaml

kubectl create -f taskmanager-deployment.yaml

kubectl create -f jobmanager-service.yaml

You can then access the Flink UI via kubectl proxy:

Run kubectl proxy in a terminal

Navigate to 

http://localhost:8001/api/v1/proxy/namespaces/default/services/flink-jobmanager:8081 

in your browser

View the attached .yaml file


