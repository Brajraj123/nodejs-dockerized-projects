pipeline {
    agent any
    tools {
        nodejs "NodeJS"
    }
    stages {
        stage("checkout") {
            steps {
                checkout scm
            }
        }

        stage("Test") {
            steps {
                sh 'npm test'
            }
        }

        stage("Build") {
            steps {
                sh 'npm run build'
            }
        }

        stage("Build Image"){
            steps {
                sh 'docker build -t my-node-app:1.0 .'
            }
        }

        stage("Docker Push") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_cred', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]){
                    sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
                    sh 'docker tag my-node-app:1.0 brajrajsingh751/demo-aap:1.0'
                    sh 'docker push brajrajsingh751/demo-aap:1.0'
                    sh 'docker logout'
                }
            }
        }
    }
    
    post {
        success {
            emailext body: 'Your pipeline was successful.',
                     subject: 'Pipeline Success Notification',
                     to: 'brajrajsingh751@gmail.com'
        }
        failure {
            emailext body: 'Your pipeline failed.',
                     subject: 'Pipeline Failure Notification',
                     to: 'brajrajsingh751@gmail.com'
        }
    }
}
