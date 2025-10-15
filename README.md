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

Docker images is created

<img width="782" height="67" alt="image" src="https://github.com/user-attachments/assets/f0cd1d2f-e605-43c4-bfe7-ebd65311797e" />

We are able to open the image on port 8080. Our application is running locally

<img width="692" height="228" alt="image" src="https://github.com/user-attachments/assets/bcb6187e-2f7c-438f-8567-b64ac01a8b7f" />


We noticed that our application is blank because we didn't provide in our dockerile the apprpriate API. Let's search the API at https://www.themoviedb.org/

<img width="743" height="342" alt="image" src="https://github.com/user-attachments/assets/34a8d075-086e-44c4-81f9-49dbbb672c62" />


Open a web browser and navigate to TMDB (The Movie Database) website.
Click on "Login" and create an account.
Once logged in, go to your profile and select "Settings."
Click on "API" from the left-side panel.
Create a new API key by clicking "Create" and accepting the terms and conditions.
Provide the required basic details and click "Submit."
You will receive your TMDB API key.

# Integrate API

Now, let's create our new image with our API after deleting all present containers with the command:  docker build --build-arg TMDB_V3_API_KEY=<your-api-key> -t netflix .

<img width="1257" height="373" alt="image" src="https://github.com/user-attachments/assets/afb2e906-147c-4018-921a-731774bb4add" />

# Phase 2: Add security on our running application

- Install SonarQube and Trivy:

docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
To access:

<img width="905" height="136" alt="image" src="https://github.com/user-attachments/assets/c292d0d9-0795-4704-9450-a8136c520910" />


Now that Sonarqube is installed, let's access the application on publicIP:9000 (by default username & password is admin)

<img width="500" height="257" alt="image" src="https://github.com/user-attachments/assets/a890eedc-57cd-4770-8940-222f6033b3a2" />


To install Trivy:

sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy        

Trivy is installed
<img width="831" height="116" alt="image" src="https://github.com/user-attachments/assets/93125063-60e9-44ac-aee2-b488ab57ffd1" />

To scan an image, just run the command trivy image <imageid>

# Phase 3 CI/CD Set Up

- Install Jenkins
  
<img width="471" height="150" alt="image" src="https://github.com/user-attachments/assets/badc68d5-30c2-43a0-9679-a6e6e3a52d47" />

Let's check if Jenkins is accessible on port 8080

<img width="1108" height="407" alt="image" src="https://github.com/user-attachments/assets/84958b3f-1c6c-4269-ba51-e2be88c67d64" />

Open the secret Jenkins file to Generate the password. Now we have access to our Jenkins and we can install all the necessaries plugins.


- Install Necessary Plugins in Jenkins:

Goto Manage Jenkins →Plugins → Available Plugins → Install below plugins:
   
   Eclipse Temurin Installer (Install without restart), SonarQube Scanner (Install without restart), NodeJs Plugin (Install Without restart), Email Extension Plugin

<img width="767" height="232" alt="image" src="https://github.com/user-attachments/assets/830b475c-6ef1-4506-a0cd-168223b44156" />


- Configure Java and Nodejs in Global Tool Configuration
  
Goto Manage Jenkins → Tools → Install JDK(17) and NodeJs(16)→ Click on Apply and Save

- Configure SonarQube
Administration-Security-Users

<img width="837" height="225" alt="image" src="https://github.com/user-attachments/assets/8b1902a7-62ee-4bee-8ba2-f358ade6a29c" />

Create the token. Copy the token. Goto Jenkins Dashboard → Manage Jenkins → Credentials → Add Secret Text. It should look like this

<img width="862" height="212" alt="image" src="https://github.com/user-attachments/assets/099294e0-08d7-4a01-bd27-6e366c66e58a" />

The Sonar Token is created

<img width="1266" height="145" alt="image" src="https://github.com/user-attachments/assets/c1c25a6a-dc2b-4134-b426-7106cb1e98e7" />

- Configuration of Sonar token

Go to manage Jenkins-System-Add Sonarqube and paste the Sonarqube website url. After that go to Manage Jenkins-Tools-Add Sonarqube (Sonarqube Scanner)

Our pipeline is ready to be deployed


Create a Jenkins webhook

Configure CI/CD Pipeline in Jenkins:
Create a CI/CD pipeline in Jenkins to automate your application deployment.
