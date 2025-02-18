pipeline {
    agent any
    stages {
        stage('Build docker image') {
            steps {
                echo 'Building..'
            }
        }
        stage('push the docker image to docker hub') {
            steps {
                echo 'Testing..'
            }
        }
        stage('update the deployment file') {
            steps {
                echo 'Deploying....'
            }
        }
        stage('commit the deployment file changes') {
            steps {
                echo 'Committing....'
            }
        }
    }
}