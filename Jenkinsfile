pipeline {
    agent { 
        node {
            label 'docker-agent-python'
            }
      }
    triggers {
        pollSCM '* * * * *'
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('thuong-docker')
    }
    stages {
        stage('Build') {
            steps {
                echo "Building.."
                sh '''
                pip install -r requirements.txt
                '''
            }
        }
        stage('Test') {
            steps {
                echo "Testing.."
                sh '''
                python3 hello.py
                python3 hello.py --name=Brad
                '''
            }
        }
        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh '''
                 docker build -t hoaithuongdata/myapp .
                '''
            }
        }
        stage('Deliver') {
            steps {
                echo 'Deliver....'
                sh '''
                docker tag hoaithuongdata/myapp hoaithuongdata/myapp:v1
                docker push hoaithuongdata/myapp
                '''
            }
        }
    }
}
