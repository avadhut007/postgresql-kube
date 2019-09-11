# postgresql-kube

Config Maps for PostgreSQL Configurations
We will be using config maps for storing PostgreSQL related information. Here, we are using the database, user and password in the config map which will be used by the PostgreSQL pod in the deployment template.

File: postgres-configmap.yaml

#To Create Postgres config maps resource: {Instead of ConfigMap You may use k8s secret too}
> kubectl create -f postgres-configmap.yaml
> kubectl get configmap

Define the Azure Disk Storage Class:
Kubernetes can dynamically provision Azure Disks using the Azure Kubernetes integration, which was configured when UCP was installed. For Kubernetes to determine which APIs to use when provisioning storage, you must create Kubernetes Storage Classes specific to each storage backend. 
Depending on your use case, you can deploy one or both of the Azure Disk storage Classes (Standard and Advanced).

To create a Standard Storage Class:
> kubectl create -f standard-azure-volume.yaml

To determine which Storage Classes have been provisioned:
> kubectl get storageClass

Create an Azure Disk with a Persistent Volume Claim :
After you create a Storage Class, you can use Kubernetes Objects to dynamically provision Azure Disks. This is done using Kubernetes Persistent Volumes Claims. 
> kubectl create -f azure-disk-pvc.yaml
> kubectl get pvc
At this point, you should see a new Persistent Volume Claim and Persistent Volume inside of Kubernetes. You should also see a new Azure Disk created in the Azure Portal.

PostgreSQL Deployment
PostgreSQL manifest for deployment of PostgreSQL container uses PostgreSQL 10.4 image. It is using PostgreSQL configuration like username, password, database name from the configmap that we created earlier. It also mounts the volume created from the persistent volumes and claims to make PostgreSQL container’s data persists.
Create Postgres deployment:
> kubectl create -f postgres-deployment.yaml

PostgreSQL Service:
To access the deployment or container, we need to expose PostgreSQL service. Kubernetes provides different type of services like ClusterIP, NodePort and LoadBalancer.

With ClusterIP we can access PostgreSQL service within Kubernetes. NodePort gives the ability to expose service endpoint on the Kubernetes nodes. For accessing PostgreSQL externally, we need to use a Load Balancer service type which exposes the service externally.

File: postgres-service.yaml
> kubectl create -f postgres-svc.yaml

Connect to PostgreSQL:
> psql -h <DB IP> -U postgres --password -p 5432 postgresdb
  <Enter above user passwd to get in>
