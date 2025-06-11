pipeline{
    agent any
    stages{

        stage('docker push'){
            steps{
                dir('backend'){
                script {
                    withDockerRegistry(
                        credentialsId: '0bfdb452-e0b6-40bb-ac8a-ba599be5f55f', 
                        url: 'https://index.docker.io/v1/'
                    ){
                        echo "🐳 Building Docker image..."
                        sh 'docker build -t iamsumitkumar/java-project-backend:$BUILD_ID .'

                        echo "📤 Pushing Docker image..."
                        sh 'docker push iamsumitkumar/java-project-backend:$BUILD_ID'

                    } 
                }
                }
            }
        }

    }
     
}