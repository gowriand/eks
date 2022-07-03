----------------------
Basic Cluster setup instcructions:
----------------------

**Create an IAM User**
1. Create an IAM user with programmatic access and administrator-level privileges and note secret keys.

------------------------
**Launch an EC2 Instance and other command line tools**
1. Create an **EC2 instance** in the same region. eg: us-east-1
2. Install **awscli** and configure it using the credentials created
---
       sudo apt-get update
       apt install unzip
       curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
       unzip awscliv2.zip
       sudo ./aws/install
       aws --version
       aws configure
 (give keys)
 
 --------
3. Install eksctl 
   
   Ref: https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html
---
       curl -o kubectl https://s3.us-west-2.amazonaws.com/amazon-eks/1.22.6/2022-03-09/bin/linux/amd64/kubectl
       chmod +x ./kubectl
       mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
       kubectl version --short --client

4. Install kubectl 
   Ref- - https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html

---
       curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
       sudo mv /tmp/eksctl /usr/local/bin
       eksctl version
 
---------------------
**Provision an EKS Cluster**

1. EKS cluster Provisioning (using nodetype EC2 instances)
 
   Can be done through AWS Console or eksctl command.
   Here using eksctl command. 
   The input can be given in yaml or in the commandline itself.
  
   Eg 1: Using yaml- cluster.yaml  
   
   ---
       eksctl create cluster -f cluster.yaml
       eksctl get nodegroup --cluster rkc-EKS-Democlusterfromec2
   OR
   
   Eg 2: Using comand directly with sufficient options : 
   
   ---
          eksctl create cluster --name rkc-EKS-Democlusterfromec2 --region us-east-1
   
   Check cluster
   
   ---
       eksctl get cluster
       kubectl get nodes
 
 
2. Update kubecomfig -
---
       aws eks update-kubeconfig --name rkc-EKS-Democlusterfromec2 --region us-east-1

 
**Create a Service and Deployment on Your EKS Cluster**


1. Create a LoadBalancerservice.

---
       kubectl apply -f nginx-svc.yaml  
       
 Check the status of your LoadBalancerservice using kubectl.

---
       kubectl get service
2. Create a Deployment on your EKS cluster.
      Use nginx image in default Docker Hub registry.
 ---
       kubectl apply -f nginx-deployment.yaml 
       
3. Check the status of cluster, deployment, and pods

---
       kubectl get  pods
       kubectl get  rs
       
 
6. Then check using the DNS name of the LoadBalancer.


**Test the High Availability **

1. In the AWS Console, shut/terminate any worker.
2. Check the status using kubectl.
3. EKS will launch new instance to keep your service running.


Course files can be found here: https://github.com/ACloudGuru-Resources/Course_EKS-Basics
