CI/CD Pipeline Project

Overview

This project showcases a comprehensive CI/CD pipeline implementation designed to optimize the software delivery process for both backend (Spring) and frontend (Angular) applications. It integrates automation, testing, code quality assurance, and real-time monitoring to ensure efficient and reliable deployments.
Features

1. CI/CD Pipeline with Jenkins
Automates build, test, and deployment processes for backend and frontend applications.
Ensures consistent and fast delivery with streamlined pipelines.
2. Email Notifications
Configured automated email alerts to notify about pipeline status (success or failure).
3. Code Quality Assurance
Integrated SonarQube to monitor and improve code quality and security continuously.
4. Artifact Management
Utilized Nexus as a repository to manage build artifacts and dependencies securely.
5. Containerization and Orchestration
Dockerized backend and frontend applications for portability.
Used Docker Compose for managing multi-container environments.
6. Real-Time Monitoring
Integrated Prometheus and Grafana for real-time monitoring and visualization through custom dashboards.
Technologies Used

Jenkins: CI/CD automation
Git: Version control
Maven: Dependency management
JUnit/Mockito: Testing frameworks
SonarQube: Code quality checks
Nexus: Artifact repository
Docker & DockerHub: Containerization
Docker Compose: Multi-container orchestration
Prometheus & Grafana: Monitoring and visualization
Installation

Clone the Repository
git clone https://github.com/Oumayma-cherif/Devops.git  
cd Devops  
Backend Setup
Navigate to the backend directory.
Build the project using Maven:
mvn clean install  
Frontend Setup
Navigate to the frontend directory.
Install dependencies:
npm install  
Run Docker Containers
Use docker-compose to start all services:
docker-compose up -d  
Testing

Unit Testing: Run tests using JUnit/Mockito in the backend:
mvn test  
Integration Testing: Included in the Jenkins pipeline for end-to-end verification.
Monitoring

Access Prometheus at http://localhost:9090.
View detailed Grafana dashboards at http://localhost:3000.
Project Structure

Devops/  
│  
├── backend/          # Spring backend application  
├── frontend/         # Angular frontend application  
├── Jenkinsfile       # CI/CD pipeline configuration  
├── docker-compose.yml # Multi-container orchestration  
└── README.md         # Project documentation  
Future Enhancements

Implement Kubernetes for advanced orchestration.
Expand monitoring capabilities with alerts for critical metrics.
Contact

For questions or suggestions, feel free to reach out at [your email].
