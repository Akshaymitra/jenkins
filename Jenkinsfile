pipeline {
    agent any

    stages {
        stage("Start") {
            steps {
                // Checkout the main branch of the GitHub repository
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-creds', url: 'https://github.com/Akshaymitra/jenkins']])

                // Run python3 test.py
                script {
                    sh 'python3 test.py'
                }
            }
        }

        stage('Build Docker image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t utkarshns/jenkins-ts .'
                }
            }
        }
        
        stage('Push Docker image') {
            steps {
                script {
                   withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'dockerpass', usernameVariable: 'dockeruser')]) {
                        sh'docker login -u ${dockeruser} -p ${dockerpass}'
}
                        sh'docker push utkarshns/jenkins-ts'
                 
                }
            }
        }
    
        stage('Deploy to K8s') {
            steps {
                script {
                    // Build the Docker image
                    sh 'kubectl --kubeconfig=/home/ubuntu/.kube/config get pods'
                }
            }
        }

    }
    
}
