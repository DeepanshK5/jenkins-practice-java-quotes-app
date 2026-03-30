pipeline{
    agent{label 'newagent'}
    stages{
        stage('Code'){
            steps{
                git url:'https://github.com/DeepanshK5/jenkins-practice-java-quotes-app',branch:'master'
            }
        }
        stage('Build'){
            steps{
                sh 'docker build -t quotes-app:latest .'
            }
        }
        stage('Push'){
            steps{
                withCredentials([usernamePassword(
                    'credentialsId':"dockerhub-cred",
                    'passwordVariable':"dockerhub_pass",
                    'usernameVariable':"dockerhub_user"
                )]){
                    sh "docker login -u ${env.dockerhub_user} -p ${env.dockerhub_pass}"
                    sh "docker image tag quotes-app:latest ${env.dockerhub_user}/quotes-app:latest"
                    sh "docker push ${env.dockerhub_user}/quotes-app:latest"
                }
            }
        }
        stage('Deploy'){
            steps{
                sh 'docker compose up -d'
            }
        }
    }
}
