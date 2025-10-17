pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'jubelshaji'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/JubelShaji/CI-CD-project.git'
            }
        }

        stage('Install Dependencies & Test Backend') {
            steps {
                dir('backend') {
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }

        stage('Install Dependencies & Test Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm test -- --coverage --watchAll=false'
                }
            }
        }

        stage('Build & Push Docker Images') {
            parallel {
                stage('Backend Image') {
                    steps {
                        script {
                            dir('backend') {
                                def backendImage = docker.build("${DOCKER_REGISTRY}/jenkin-docker:backend-${IMAGE_TAG}")
                                docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                                    backendImage.push()
                                    backendImage.push('backend-latest')
                                }
                            }
                        }
                    }
                }
                stage('Frontend Image') {
                    steps {
                        script {
                            dir('frontend') {
                                def frontendImage = docker.build("${DOCKER_REGISTRY}/jenkin-docker:frontend-${IMAGE_TAG}")
                                docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                                    frontendImage.push()
                                    frontendImage.push('frontend-latest')
                                }
                            }
                        }
                    }
                }
            }
        }
        stage('Deployment') {
            steps {
                script {
                     sh """
                        sed -i 's|jubelshaji/jenkin-docker:backend-latest|${DOCKER_REGISTRY}/jenkin-docker:backend-${IMAGE_TAG}|g' docker-compose.yaml
                        sed -i 's|jubelshaji/jenkin-docker:frontend-latest|${DOCKER_REGISTRY}/jenkin-docker:frontend-${IMAGE_TAG}|g' docker-compose.yaml
                     """
                     sh 'docker compose stop mongo backend frontend'
                     sh 'docker compose rm -f mongo backend frontend'
                     sh 'docker compose up -d mongo backend frontend'
                }
            }
        }
    }

    post {
        success { echo '🎉 Pipeline completed successfully!' }
        failure { echo '❌ Pipeline failed!' }
        always { sh 'docker system prune -f' }
    }
}
