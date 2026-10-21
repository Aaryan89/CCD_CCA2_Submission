pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Aaryan89/CCD_CCA2_Submission.git'
            }
        }
        
        stage('Test (File Verification)') {
            steps {
                // Fulfills the testing requirement by verifying core files exist
                sh 'test -f index.html || (echo "Missing index.html" && exit 1)'
                sh 'echo "Static files verified."'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t harshadpardhi/docs-site:latest .'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push harshadpardhi/docs-site:latest'
                }
            }
        }

        stage('Minikube Deployment') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}