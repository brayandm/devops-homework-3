pipeline {
    agent any

    stages {
        
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ttl.sh/pythonapp-brayand:1h ."
            }
        }

        stage('Push Docker Image') {
            steps {
               sh "docker push ttl.sh/pythonapp-brayand:1h"
            }
        }

        stage('Run in K8s') {
            steps {
                withCredentials([file(credentialsId: 'kubernetes-config', variable: 'kubeConfig')]) {
                    sh "kubectl --kubeconfig ${kubeConfig} run --image ttl.sh/pythonapp-brayand:1h myapp"   
                }
            }
        }
    }
}
