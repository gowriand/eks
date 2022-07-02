Basic Cluster setup instcructions:
----------------
Create an IAM User
1. Create an IAM user with programmatic access and administrator-level privileges and note secret keys.
------------------------
Launch an EC2 Instance
1. Create an EC2 instance in us-east-1region.
2. Install awscli and configure it using the credentials created
 #######
 sudo apt-get update
 apt install unzip
 curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
 unzip awscliv2.zip
 sudo ./aws/install
 aws --version
 aws configure
 #####
3. Install eksctl - https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html
 ######
 curl -o kubectl https://s3.us-west-2.amazonaws.com/amazon-eks/1.22.6/2022-03-09/bin/linux/amd64/kubectl
 chmod +x ./kubectl
 mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
 kubectl version --short --client
 #########
4. Install kubectl - https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
 #########
 curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
 sudo mv /tmp/eksctl /usr/local/bin
 eksctl version
 #######
---------------------
Provision an EKS Cluster
1. Use eksctl to provision an EKS cluster.
   Use yaml- cluster.yaml or direct command
   ####
   #  eksctl create cluster -f cluster.yaml
      eksctl get nodegroup --cluster rkc-EKS-Democlusterfromec2
   OR
   
   Check cluster
     eksctl get cluster
     kubectl get nodes
  ##############3
2. Update kubecomfig -
	 # aws eks update-kubeconfig --name rkc-EKS-Democlusterfromec2 --region us-east-1
---------------------------
Create a Deployment on Your EKS Cluster
1. Create a LoadBalancerservice.
      # kubectl apply -f nginx-svc.yaml  
     Check the status of your LoadBalancerservice using kubectl.
       # kubectl get service
2. Create a Deployment on your EKS cluster.
      Use nginx image in default Docker Hub registry.
         kubectl apply -f nginx-deployment.yaml 
         kubectl get  pods
         kubectl get  rs
5. Check the status of your cluster, deployment, and pods using kubectl.
6. When the Deploymentis up and running, check that you can access your application using the DNS name of the LoadBalancer.
--------
Test the High Availability Features of Your EKS Cluster
1. In the AWS Console, shut down all the worker nodes.
2. Check the status of your cluster, deployment, and pods using kubectl.
3. After a few minutes, you should see EKS launching new instances to keep your service running.
4. When the cluster is back to a steady state, check that your application is up and running.
1. Course files can be found here: https://github.com/ACloudGuru-Resources/Course_EKS-Basics
