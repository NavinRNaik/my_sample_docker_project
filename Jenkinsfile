pipeline {
    agent {
        label 'docker-enabled-agent'
    }

    environment {
        DOCKER_REGISTRY = "docker.io"
        DOCKER_REPO = "navinrnaik/sampleapp"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    tools {
        maven 'Maven_3_9'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Maven Build') {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Unit Tests') {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh """
                        docker build -t ${DOCKER_REPO}:${IMAGE_TAG} .
                    """
                }
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                                                 usernameVariable: 'DOCKER_USER',
                                                 passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    """
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh """
                    docker push ${DOCKER_REPO}:${IMAGE_TAG}
                    docker tag ${DOCKER_REPO}:${IMAGE_TAG} ${DOCKER_REPO}:latest
                    docker push ${DOCKER_REPO}:latest
                """
            }
        }

        stage('Cleanup Workspace & Docker') {
            steps {
                sh """
                    docker rmi ${DOCKER_REPO}:${IMAGE_TAG} || true
                    docker rmi ${DOCKER_REPO}:latest || true
                    mvn clean
                """
            }
        }
    }

    post {
        success {
            echo "Build and Docker image push successful."
        }
        failure {
            echo "Pipeline failed. Please review logs."
        }
    }
}
