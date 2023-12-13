# EKS-with-autoscaller-project

## Overview

In this project i used AWS EKS with auto scaller which allow to manage the capacity of the app ,helping  to maintain the performance and reduce cost, by reducing the number of worker nodes or increasing it depending on the load on the workloads 


## Creation  Steps

### 1. Creating VPC using CloudFormation stack 
we create the required resources  using CloudFormation stack. 

### 2. Create a Role with "AmazonEKSCluster" Policy to give the required permission for EKS   
### 3. Creating EKS Cluster through these steps

1.choose the vpc you have created and make sure the required subnets are chosen.
2.choose the role you have created for eks.
3.choose security group which has been created using the template of vpc.
4.Public and private cluster endpoint access settings.
5.keep the rest settings as default( or you can change them depending on your preferences).

### 4. Node Group Creatoin Steps
- create role for ec2 with policies (EC2ContainerRegistry, EKS_CNI, EKSWorkerNode, IAM-Policy_for_AutoScaler) and choose it while creating node group .
- choose the type of instance.
- Make sure you chose the VPC you have chosen for eks cluster.
- choose the number of minimum ,desired,desired instances(for example make them :1,1,2)

### 5. Connecting to EKS 
First,you can use your termianl or Cloud Shell for this.
```bash
aws configure 
```
Then,execute the following command to configure kubectl for interacting with EKS cluster:

```bash
aws eks update-kubeconfig --name <cluster_name>
```
note: you will find commands.sh files, you can use these commands in it to validate that the creation and working of workloads for this and upcoming step.
### 6. Deploy cluster autoscaller
First,we add policy to the role we created for node group to enable it to autoscale the instances 
then,modify the file with your eks cluster name and make sure that the version of eks cluster the same as eks which you created ,and deploy autoscaller using:
```bash
kubectl apply -f cluster-AutoScaler.yaml
```
### 7.Deploy sample app(nginx)
run this command to deploy the app and create a loadbalancer which we will use to access nginx:
```bash
kubectl apply -f  nginx.yaml
```
### 8.Increase the replicas of nginx deployemnt
To test that the autoscaling is doing its job edit the deployemnt of nginx and increase the number of replics for example for 15.
you will notice that a new node is being created to be able to match this demand.
