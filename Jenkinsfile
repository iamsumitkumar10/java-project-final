pipeline {
    agent any
    environment {
        KUBECONFIG = '/home/sumit/.kube/config'
    }

    stages {
        stage("git checkout") {
            steps {
                git branch: 'main', 
                    credentialsId: 'ed22fdee-75de-49be-b696-549a31ed6c68', 
                    url: 'https://github.com/iamsumitkumar10/java-project-final.git'
            }
        }


        stage('docker push backend') {
            steps {
                dir('backend') {
                    script {
                        withDockerRegistry(
                            credentialsId: '0bfdb452-e0b6-40bb-ac8a-ba599be5f55f',
                            url: 'https://index.docker.io/v1/'
                        ) {
                            echo "🐳 Building Docker image..."
                            sh 'docker build -t iamsumitkumar/java-project-backend:$BUILD_ID .'
                            echo "📤 Pushing Docker image..."
                            sh 'docker push iamsumitkumar/java-project-backend:$BUILD_ID'
                        }
                    }
                }
            }
        }

        stage('docker push frontend') {
            steps {
                dir('frontend') {
                    script {
                        withDockerRegistry(
                            credentialsId: '0bfdb452-e0b6-40bb-ac8a-ba599be5f55f',
                            url: 'https://index.docker.io/v1/'
                        ) {
                            echo "🐳 Building Docker image..."
                            sh 'docker build -t iamsumitkumar/java-project-frontend:$BUILD_ID .'
                            echo "📤 Pushing Docker image..."
                            sh 'docker push iamsumitkumar/java-project-frontend:$BUILD_ID'
                        }
                    }
                }
            }
        }

        stage('docker-compose deploy backend') {
            steps {
                dir('backend') {
                    script {
                        echo "🚀 Deploying backend with minikube ..."
                        // sh "BUILD_ID=$BUILD_ID docker-compose down || true"
                        // sh "BUILD_ID=$BUILD_ID docker-compose up -d"
                        sh 'kubectl apply -f backend-deployment.yaml'
                        sh 'kubectl apply -f backend-service.yaml'
                        
                    }
                }
            }
        }

        stage('docker-compose deploy frontend') {
            steps {
                dir('frontend') {
                    script {
                        echo "🚀 Deploying frontend with minikube ..."
                        // sh "BUILD_ID=$BUILD_ID docker-compose down || true"
                        // sh "BUILD_ID=$BUILD_ID docker-compose up -d"

                        sh 'kubectl apply -f frontend-deployment.yaml'
                        sh 'kubectl apply -f frontend-service.yaml'
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully.'
            emailext(
                subject: 'this mail from java-project-final build id ${BUILD_ID}',
                body: '''Build is successful.
Build URL: ${BUILD_URL}
                ''',
                from: 'sumitkumar703327@gmail.com',
                to: 'sumitku3500@gmail.com'
            )
        }

        failure {
            echo '❌ Pipeline failed. Check the logs for details.'
            emailext(
                subject: 'this mail from java-project-final build id ${BUILD_ID}',
                body: '''Build failed.
Build URL: ${BUILD_URL}
                ''',
                from: 'sumitkumar703327@gmail.com',
                to: 'sumitku3500@gmail.com'
            )
        }

        always {
            echo '📋 Pipeline execution finished.'
        }
    }
}
