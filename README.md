Udacity Capstone Project 5 Kemal Ozturk

Steps

•	Launch an instance for Jenkins
•	Install Jenkins
•	Install Plugins on Jenkins --Blue Ocean --AWS Plugin
•	Install aws-cli,eksctl,kubectl
•	Create Cluster and Nodes
•	Dockerfile
•	Jenkinsfile
•	Pipeline



Jenkins

 

http://ec2-54-146-11-56.compute-1.amazonaws.com:8080/

 

AWS EKS

eksctl create cluster --name ozturqcapstonecls --version 1.17 --nodegroup-name ozturq-lnodes --node-type t2.small --nodes 2 --nodes-min 1 --nodes-max 3 --node-ami auto --region us-east-1

 

EKS Ready message

EKS cluster "ozturqcapstonecls" in "us-east-1" region is ready
 
 
Website
http://aa9e9f355138c41c5a48a6fff0b01565-1704206919.us-east-1.elb.amazonaws.com:8000/

 
