pipeline{
    agent any

    stages {

        stage('Build Docker Image'){
            steps{
                script{
                    dockerapp = docker.build("mfguimaraes/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
        
        stage ('Push Docker Image'){
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com','dockerhub'){
                    dockerapp.push('latest')
                    dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage ('Deploy Kubernets'){
            steps{
                withKubeConfig([credentialsId: 'kubeconfig']){
                    sh 'kubeclt apply -f ./k8s/deployment.yml'

                }
            }
        }

    }

}