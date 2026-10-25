pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    environment {
        PATH = "/usr/local/bin:/opt/homebrew/bin:/Applications/Docker.app/Contents/Resources/bin:${env.PATH}"
    }

    stages {

        stage('Get Code') {
            steps {
                git branch: 'main', url: 'https://github.com/kamakshi2113/docker-jenkins-project.git'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t my-web-app .'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh 'docker rm -f my-web-container || true'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d --name my-web-container -p 8081:80 my-web-app'
            }
        }

        stage('Check Website') {
            steps {
                sh 'sleep 3'
                sh 'curl http://localhost:8081'
            }
        }
    }

    post {
        success {
            echo 'Done! Open http://localhost:8081 to see the website'
        }
        failure {
            echo 'Something went wrong, check the logs'
        }
    }
}
