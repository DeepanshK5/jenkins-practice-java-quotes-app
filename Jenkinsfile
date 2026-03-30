@Library("Shared") _
pipeline{
    agent{label 'newagent'}
    stages{
        stage('Hello'){
            steps{
                script{
                    hello()
                }
            }
        }
        stage('Code'){
            steps{
                script{
                    clone('https://github.com/DeepanshK5/jenkins-practice-java-quotes-app','master')
                }
            }
        }
        stage('Build'){
            steps{
                script{
                    docker_build('quotes-app','latest')
                }
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
