#  Netflix-Clone-on-AWS

<img width="1020" height="465" alt="image" src="https://github.com/user-attachments/assets/c03fa650-aef5-4312-9d13-390befe1fb22" />







In this project, we will deploy the Netflix clone on AWS using tools like **Jenkins for CICD, Prometheus and Grafana for monitoring, Trivy and Sonarqube for security, Argocd and Helm to deploy the app on Kubernetes**. 

**Project Architecture**: We will start by creating an EC2 instance and run our application locally using Docker container. When the application is running, we will integrate manually security with SonarQube and Trivy. When it's done, we will integrate this process in Jenkins. Our docker image will be created and stored in Github. After that, we integrate Prometheus and Grafana to monitor EC2 instances. We will be able to get email notification on how much cpu is used, how many job is failed or succeed. The application will be deployed on Kubernetes using ArgoCd (GitOps tool) and monitor our kuberenetes cluster through Helm chart.


## Phase 1: Initial Set Up and Deployment

 - Provision an EC2 instance on AWS 

<img width="782" height="201" alt="image" src="https://github.com/user-attachments/assets/d76567ce-53ef-46de-aad1-cbf9ede0cdfa" />

We decided to attach this server an EIP

<img width="850" height="306" alt="image" src="https://github.com/user-attachments/assets/4f8514aa-fbad-4f2e-bac7-85dd36961ff9" />

 Connect the instance using SSH
 
<img width="410" height="196" alt="image" src="https://github.com/user-attachments/assets/b2784c53-40a1-4888-b256-02d8d1cd5ed0" />

- Update and install package in the server
   Command: sudo apt update        
            sudo apt upgrade -y
  
<img width="571" height="247" alt="image" src="https://github.com/user-attachments/assets/63f7fa92-e8b5-4f31-8eb7-fdb3838635b0" />

- Clone the repository with git clone and  Access the repository and enter in our Netflix Devsecops folder
  
- Install Docker
  <img width="728" height="203" alt="image" src="https://github.com/user-attachments/assets/2260aca7-0756-44bd-997e-07a5e3f99022" />
















