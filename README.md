# 1. Cloud Infrastructure & Deployment (Azure)

Task Overview:

Set up a simple web application using Node.js and deploy it on Azure App Service or an Azure Virtual Machine. The application should be publicly accessible via a URL.

Solution Overview:

The web application was deployed on Azure using Azure App Service. The application is configured to run Node.js, and it is accessible via a public URL.

Architecture Diagram:

<img width="1173" alt="Screenshot 2025-02-05 at 9 12 06 PM" src="https://github.com/user-attachments/assets/4de9c375-3918-4ab3-8bc3-4be98451ebaa" />


Steps to Deploy the Application:
	1.	Create an Azure Account and Log in:
		Ensure you have an active Azure subscription.
  
	2.	Create a Resource Group:
	•	In Azure Portal, navigate to Resource Groups.
 
	•	Click Add and provide a name for the resource group (cicd-pipeline_group).
 
	•	Select a region Central India (zone 1), then click Review + Create.
 
	3. Create an Azure Virtual Machine:
	•	In Azure Portal, navigate to Virtual Machines and click Add.
 
	•	Select the Resource Group and configure the VM settings (image, size, authentication).
 
	•	Create and ensuring proper network security rules (e.g., open port 80, 8080,9000,3000,5173 etc).
 
	4. Set Up Docker on the Azure VM:
 
	5. Install Jenkins on the VM:
 
	1.	Install Jenkins on the VM:
 
	•	Follow installation steps for your VM’s OS (Ubuntu example: sudo apt install openjdk-17-jdk && sudo apt install jenkins).
 
	2.	Start Jenkins:
 
	•	Access Jenkins via http://98.70.55.30:8080.
 
	6. Integrate Jenkins with GitHub:
 
	•	GitHub Webhook: In your GitHub repository, add a webhook pointing to Jenkins.
 
	•	Jenkins Configuration: Set up a Pipeline Job in Jenkins with GitHub repository details and configure the pipeline using a Jenkinsfile for CI/CD.
	7. Deploy the Application with Jenkins:
 
	•	Set up the Jenkins pipeline to build, test, and deploy my Todo-app.
 
	•	On push to GitHub, the webhook triggers Jenkins to pull the code, Do docker compose up, and deploy it to the Azure VM.
 
	8. Access the Application:
 
	•	After successful deployment, access your app through the public IP of the Azure VM.
 
  <img width="1280" alt="Screenshot 2025-02-05 at 1 25 15 PM" src="https://github.com/user-attachments/assets/fdd23e98-8937-4d7e-badf-8949d7bf6dae" />

This summary outlines the steps for deploying a Todo-app using Azure VM, Docker, and Jenkins, with the CI/CD process set up for automation.
Azure Configurations Used:

	•	Resource Group: cicd-pipeline_group
 
	•	App Service: Todo-app
 
	•	App Service Plan: Azure for Students
 
	•	Region: Central India(zone 1)
 
	•	Runtime Env: Node.js
 
	•	Deployment Source: GitHub
 
	•	Public URL: [https://my-nodejs-webapp.azurewebsites.net](http://98.70.55.30:5173/)

Screenshots of a Successful Deployment:
<img width="1280" alt="Screenshot 2025-02-05 at 2 36 24 PM" src="https://github.com/user-attachments/assets/92d6a41b-7c7b-4e80-8180-56da303fa2e8" />
2. CI/CD Pipeline Implementation

Task Overview:

Create a CI/CD pipeline to automate the deployment of the Todo-app web application using Jenkins.

Solution Overview:

I implemented a Jenkins CI/CD pipeline that automates the deployment of the web application to Azure App Service. This pipeline includes stages for building, testing, and deploying the application.

Explanation of Different Pipeline Stages:

 1.	Build Stage:

 •	Checkout Repository: The pipeline begins by checking out the latest code from the GitHub repository.

 •	Set Up Docker: The Docker environment is set up for building the application image.

 •	Build Docker Image: Using Docker Compose, the pipeline builds a Docker image of the application.

 •	Install Dependencies: Inside the Docker container, dependencies are installed using npm install.

 •	Run Tests: The tests are executed using npm test to ensure that the application is stable and working as expected.

 2.	Quality Analysis Stage (SonarQube Integration):

 •	Scan with SonarQube: The code is scanned using SonarQube for quality checks, including static code analysis and detection of bugs or vulnerabilities.

 3. 	Deploy Stage (Using Jenkins, Docker Compose, and Azure VM):

 •	Run Docker Container: In this stage, Jenkins triggers the execution of the Docker Compose file to bring up the application containers on the Azure VM using the command: docker-compose down to stop any existing containers and docker-compose up -d to start new ones.

 •	Deploy to Azure VM: The final deployment step deploys the application to the Azure Virtual Machine using Docker Compose, ensuring the app runs in a consistent environment across deployments.
 
How Environment Variables/Secrets are Managed:

Jenkins Credentials Store: Jenkins provides a Credentials Manager that securely stores sensitive data like usernames, passwords, tokens, and publish profiles.
These credentials were added under Jenkins → Manage Jenkins → Manage Credentials.
The credentials are stored in a secure manner and are used in the pipeline to interact with Azure, GitHub, and SonarQube.
  
<img width="1280" alt="Screenshot 2025-02-05 at 2 46 05 PM" src="https://github.com/user-attachments/assets/4e848de0-6107-4cdc-9e8d-56b892751196" />
<img width="1280" alt="Screenshot 2025-02-05 at 2 46 17 PM" src="https://github.com/user-attachments/assets/ca69de70-0538-445b-86b0-a42294fc37a6" />


<img width="1280" alt="Screenshot 2025-02-05 at 2 47 19 PM" src="https://github.com/user-attachments/assets/8a64577e-5034-471d-99ac-8366e6751f5d" />


<h2>3. Security & Compliance (ISO, GDPR, SOC 2)</h2>

Security Risks in DevOps & Mitigation

1. Insecure Infrastructure as Code (IaC)
 Risk: Flawed IaC can lead to unauthorized access, data breaches, and system downtime.
 Mitigation:
• Code Reviews & Automated Scanning
• Version Control & Least Privilege Access
• Compliance: ISO 27001 (A.12.5), GDPR (Art. 32), SOC 2

2. Leaky Secrets Management
 Risk: Exposed API keys & credentials lead to unauthorized access.
 Mitigation:
• Secrets Management Tools & Environment Variables
• Avoid Hardcoding & Rotate Secrets Regularly
• Compliance: ISO 27001 (A.12.1), GDPR (Art. 32), SOC 2

3. Vulnerable Dependencies
 Risk: Outdated libraries introduce security threats.
 Mitigation:
• Dependency Scanning & Regular Updates
• Software Composition Analysis (SCA)
• Compliance: ISO 27001 (A.14.2), GDPR (Art. 32)

<h1>5. Database & Storage Optimization (PostgreSQL)</h1>

1. Indexing for Faster Queries

 Issue: Full table scans slow down queries.
  Solution:
• Use B-Tree Index for lookups
• Before: SELECT * FROM users WHERE email = 'user@example.com'; (Slow)
• After: CREATE INDEX idx_email ON users (email); (Fast lookup)

2. Query Optimization

  Issue: Slow queries due to poor joins & unnecessary data fetching.
  Solution:
• Use EXPLAIN ANALYZE for performance insights
• Avoid SELECT *, index foreign keys
• Before: Unindexed JOIN (Slow)
• After: Indexed JOIN (Faster retrieval)

3. Data Partitioning for Large Tables

  Issue: Full-table scans slow down queries on large datasets.
  Solution:
• Use Partitioning:
• Range Partitioning (e.g., orders by year)
• List Partitioning (e.g., users by country)
• Hash Partitioning (even data distribution)

Example:
 CREATE TABLE orders PARTITION BY RANGE (order_date);
 CREATE TABLE orders_2024 PARTITION OF orders 
 FOR VALUES FROM ('2024-01-01') TO ('2024-12-31');
