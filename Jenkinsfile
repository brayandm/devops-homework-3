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
                sh """
                   mkdir -p .bin
                   cd .bin
                   curl -LO  https://dl.k8s.io/release/v1.27.2/bin/linux/amd64/kubectl
                   chmod +x ./kubectl
                """
                withCredentials([file(credentialsId: 'kubernetes-config', variable: 'kubeConfig')]) {
                    sh ".bin/kubectl --kubeconfig ${kubeConfig} delete --ignore-not-found=true --namespace brayan pod myapp"
                    sh ".bin/kubectl --kubeconfig ${kubeConfig} apply -f k8s/myapp.yaml"
                }
            }
        }
    }
}
