# knote-java ptoject

Simple Spring Boot app to take notes
![image](https://user-images.githubusercontent.com/107158398/183525592-77d87bd2-61f8-4af5-9152-e853b64fdb28.png)


# What do you need to automate CI and CD an application?
## I- Installing jenkins:
### After testing the code locally , the second thing to do is to setup your Jenkins pipeline.
### 1- Setup Jenkins pipeline:
#### - Install jenkins on a server 
### 2 - Steps to install jenkins : 
#### - Install jenkins Dependencies: Java JDK & JAVA JRE
#### - Download Jenkins binaries from Jenkins.io
#### - Install and Start Jenkins
https://github.com/devopstrainingschool/Jenkins-installation-steps

## II- CI
### When the developer pushes the code, jenkins will pull that code.
### Jenkins compile the code , and test it with Sonarqube, then build and push the docker image to the artifactory(docker hub).
## In order to archive this:
#### - we need to identify the type of application: Java Application , & maven compiler
#### - Build a Jenkinfile: 
https://github.com/devopstrainingschool/java-maven-app-project/blob/main/Jenkinsfile
#### - Install git : yum install git -y
#### - Install Docker
https://github.com/devopstrainingschool/docker-installation
#### - Go to Jenkins server, and create a pipeline Job.
#### - Setup maven-yaya tool: 
#### - Installing plugins: git , docker, maven
#### - We run our pipeline to make sure that maven is working properly
#### - Install Sonarqube Server
https://github.com/devopstrainingschool/sonarqube-installation
#### - Integrate Sonarqube with Jenkins
https://github.com/devopstrainingschool/sonarqube-jenkins-integration
##### - Go to Jenkins and Install sonarqube plugins
##### - GO to Jenkinsfile and add Sonarqube stages
##### - Run the Jenkins pipeline again
##### - Add docker build and docker push in the Pipeline.
##### - We need to add docker hub (artifactory ) credentials
##### - Run the Jen kins pipeline again
##### - the pipeline may failed : run this command
```
chown jenkins:docker  /var/run/docker.sock
```
##### - Then re run the pipeline
## III- Webhook between Github and Jenkins
### 1- Go to the Jenkins Job (project1), then click on configure, then look for build triggers, finally click on " GitHub hook trigger for GITScm polling"
![image](https://user-images.githubusercontent.com/107158398/184759813-e61d50e9-e077-4b80-a4ea-d91869772e35.png)

### 2- go Github , then click on the project repository, click on settings, click webhooks, then click on add a webhook
### payload = jenkins_url(ip address)/github-webhook/
![image](https://user-images.githubusercontent.com/107158398/184759249-b3f1f524-a272-417c-8487-bc0e0067c80c.png)

### then click on add webhook

### To make that the webhook is working properly , let change something on the code repository that contains jenkinsfile , and see how Jenkins server will detect it and run the pipeline automatically.
# CD part:
## I- Access to a kubernetes cluster
### To connect to a remote cluster, we need to install kubeconfig.
### Follow these steps to setup kubeconfig for the first time( Only the first):
https://github.com/devopstrainingschool/Connecting-to-kubernetes-cluster
### Install Kubectl by following these steps:
https://github.com/devopstrainingschool/kubernetes-kops-installation
### Make sure that you have git installed

## II- Create a new repository for the yaml files"
### 1- Create a repository named k8s-project1
### For each teamplate, create a yaml file and copy and paste the proper configuration
https://github.com/devopstrainingschool/kubernetes-yaml-files-template
### 2- Clone the repository locally
## III- To deploy our yaml files manually
### Run the command as follow:
```
kubectl apply -f pathtothe yamlfiles
```
#### for example
```
kubectl apply -f k8s-project1
```
## IV- To delete the deployment, please run the same command but remove apply and put delete
```
kubectl delete -f k8s-project1
```
### The issue with this is that we are deploying manually.
### to fix this we will install argo CD in our cluster.
https://github.com/devopstrainingschool/Argo-CD-Installation
### Follow the steps of the video to install application using Argo CD: follow 08-24-2022_class.
