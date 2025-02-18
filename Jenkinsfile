pipeline {
    agent any
    stages {
        stage('Build docker image') {
            steps {
                echo "init environment"
                sh 'export PATH=$PATH:/usr/bin'
                sh 'docker image build -t pythoncpp/mywebapp:$BUILD_NUMBER .'
            }
        }
        stage('push the docker image to docker hub') {
            steps {
                echo "Pushing the docker image to docker hub"
                withCredentials([string(credentialsId: 'DOCKER_HUB_TOKEN', variable: 'DOCKER_HUB_TOKEN')]) {
                    sh 'echo $DOCKER_HUB_TOKEN | docker login -u pythoncpp --password-stdin'
                    sh 'docker image push pythoncpp/mywebapp:$BUILD_NUMBER'
                }
            }
        }
        stage('update the deployment file') {
            steps {
                echo 'Updating the deployment file'
                sh 'sed -i "s/mywebapp:.*/mywebapp:$BUILD_NUMBER/g" deployment.yaml'
            }
        }
        stage('commit the deployment file changes') {
            steps {
                echo 'Committing the deployment file changes'
                withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')]) {
                    sh 'git config --global user.email "pythoncpp@gmail.com"'
                    sh 'git config --global user.name "pythoncpp"'
                    sh 'git add deployment.yaml'
                    sh 'git commit -m "updated deployment file with version as $BUILD_NUMBER"'
                    sh 'git push https://$GITHUB_TOKEN@github.com/pythoncpp/mywebapp HEAD:main'

                }
            }
        }
    }
}