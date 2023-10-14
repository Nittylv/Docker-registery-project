# Docker-registery-project
Step 1: Set Up AWS CLI and AWS IAM Authenticator
Install AWS CLI: Follow the AWS CLI installation guide for your operating system: https://aws.amazon.com/cli/

Install AWS IAM Authenticator:
curl -o aws-iam-authenticator https://amazon-eks.s3.us-east-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/aws-iam-authenticator
chmod +x ./aws-iam-authenticator
sudo mv ./aws-iam-authenticator /usr/local/bin/aws-iam-authenticator


Step 2: Create an IAM Role for EKS
Create an IAM policy for EKS access (eks-policy.json):
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "eks:DescribeCluster",
        "eks:ListClusters",
        "eks:AccessKubernetesApi",
        "eks:CreateCluster",
        "eks:UpdateClusterVersion",
        "eks:DeleteCluster"
      ],
      "Resource": "*"
    }
  ]
}


create AWS user with access to get AWS credentials 


Create the IAM role using the AWS CLI:
 aws iam create-role --role-name eks-docker-role--assume-role-policy-document file://eks-docker-regis.json
 


Although i created my roles and assigned policies on aws console



- Roles was created for EKS(kubernetes service account) to interact with other aws services/resources securely
- policy(set of permissions) was created to specify what these aws resources/services can within the eks cluster
- so basically assign these permissions to the roles(IAM roles or users)
kuberbetes workload/resources running on eks use these roles to interact aws resources







Create the EKS cluster using eksctl:
- eksctl create cluster
  name of cluster 


Step 4: Connect to the EKS Cluster
Configure kubectl to use the EKS cluster:
aws eks --region <aws-region> update-kubeconfig --name <cluster-name>









minikube start --driver=docker
- write a deployment and service file for docker registery
-specify the nodeport, i used 30500 target port and service port
-docker registery port listens on port 5000 by defaul, so servcie port was set to 5000 to match container port(target port)



kubectl create deployment docker-registery --image=k8s.gcr.io/echoserver:1.4
kubectl expose deployment docker-registery --type=NodePort --port=80

kubectl port-forward service/docker-registry-service 30699:8080

