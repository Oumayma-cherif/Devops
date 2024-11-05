pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'  // Ensure JAVA_HOME is defined in Jenkins
        maven 'M2_HOME'  // Ensure M2_HOME is defined in Jenkins
    }

    environment {
        NEXUS_CREDENTIALS = credentials('nexus-credentials')
        GITHUB_CREDENTIALS = credentials('github')
        SONAR_CREDENTIALS = credentials('admin')
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub')
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main',
                credentialsId: 'GITHUB_CREDENTIALS',
                url: 'https://github.com/Oumayma-cherif/ski.git'
            }
        }

        stage("Build Application") {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage("SonarQube Analysis") {
            steps {
                script {
                    // Start SonarQube service if not running
                    sh 'docker start sonarqube || echo "SonarQube already running."'

                    // Wait for SonarQube to be operational
                    sh '''
                    echo "Waiting for SonarQube to be operational..."
                    until curl -s http://localhost:9000/api/system/status | grep -q "UP"; do
                        echo "SonarQube is not ready, waiting for 10 seconds..."
                        sleep 10
                    done
                    echo "SonarQube is now ready!"
                    '''

                    // Run SonarQube analysis
                    sh "mvn sonar:sonar -Dsonar.login=${SONAR_CREDENTIALS_PSW} -Dsonar.host.url=http://localhost:9000 -Dsonar.projectKey=gestion-station-ski"
                }
            }
        }

        stage("Nexus Deployment") {
            steps {
                script {
                    // Start Nexus service if not running
                    sh 'docker start nexus || echo "Nexus already running."'

                    // Wait for Nexus to be operational
                    sh 'sleep 10'

                    // Deploy to Nexus
                    sh "mvn clean deploy -DskipTests -s /var/lib/jenkins/.m2/settings.xml"
                }
            }
        }

        stage("Build Docker Image") {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t oumaymacherif/gestion-station-ski:${IMAGE_TAG} ."
                }
            }
        }

        stage("Publish Docker Image") {
            steps {
                script {
                    // Log in to Docker Hub and push the image
                    docker.withRegistry('https://registry.hub.docker.com', 'DOCKER_HUB_CREDENTIALS') {
                        sh "docker push oumaymacherif/gestion-station-ski:${IMAGE_TAG}"
                    }
                }
            }
        }

        stage("Start Services with Docker Compose") {
            steps {
                script {
                    // Start the application using Docker Compose
                    sh "docker-compose up -d"
                }
            }
        }

        stage("API Testing") {
            steps {
                sh "curl -X POST http://localhost:8089/api/piste/add"
                sh "curl -X GET http://localhost:8089/api/piste/all"
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
