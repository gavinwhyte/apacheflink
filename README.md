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
Navigate to http://localhost:8001/api/v1/proxy/namespaces/default/services/flink-jobmanager:8081 in your browser


jobmanager-deployment.yaml

apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: flink-jobmanager
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: flink
        component: jobmanager
    spec:
      containers:
      - name: jobmanager
        image: flink:latest
        args:
        - jobmanager
        ports:
        - containerPort: 6123
          name: rpc
        - containerPort: 6124
          name: blob
        - containerPort: 6125
          name: query
        - containerPort: 8081
          name: ui
        env:
        - name: JOB_MANAGER_RPC_ADDRESS
          value: flink-jobmanager
taskmanager-deployment.yaml

apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: flink-taskmanager
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: flink
        component: taskmanager
    spec:
      containers:
      - name: taskmanager
        image: flink:latest
        args:
        - taskmanager
        ports:
        - containerPort: 6121
          name: data
        - containerPort: 6122
          name: rpc
        - containerPort: 6125
          name: query
        env:
        - name: JOB_MANAGER_RPC_ADDRESS
          value: flink-jobmanager
jobmanager-service.yaml

apiVersion: v1
kind: Service
metadata:
  name: flink-jobmanager
spec:
  ports:
  - name: rpc
    port: 6123
  - name: blob
    port: 6124
  - name: query
    port: 6125
  - name: ui
    port: 8081
  selector:
    app: flink
    component: jobmanager