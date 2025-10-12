# Netflix-Clone-on-AWS
In this project, we will deploy the Netflix clone on AWS using tools like **Jenkins for CICD, Prometheus and Grafana for monitoring, Trivy and Sonarqube for security, Argocd and Helm to deploy the app on Kubernetes**. 

**Project Architecture**: We will start by creating an EC2 instance and run our application locally using Docker container. When the application is running, we will integrate manually security with SonarQube and Trivy. When it's done, we will integrate this process in Jenkins and the docker image will be stored in Github. After that, we integrate monitoring with Prometheus and Grafana. The application will be deployed on Kubernetes using ArgoCd (GitOps tool) and monitor our kuberenetes cluster with Prometheus through Helm.




## Phase 1: Initial Set Up and Deployment

 - Provision an EC2 instance on AWS  and connect the instance using SSH
