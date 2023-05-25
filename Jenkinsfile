pipeline {

    agent {
        label 'my-ssh-agent-2'
    }

    tools {
        nodejs 'nodejs'
        dockerTool 'docker'
        maven 'Maven 3.9.2'
    }

    stages {

        stage('Clean') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/TodorM123/spring-with-maven'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install' //This is for building the project
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test' //This is for testing 
            }
        }

	stage('Docker login'){
           steps {
               sh 'docker login -u $DOCKERHUB_CREDENTIALS_USR  -p $DOCKERHUB_CREDENTIALS_PSW'
	   }
        }

        stage('Docker build and tag'){
            steps {
                sh 'docker build -t ${IMAGE_NAME} -f Dockerfile .'
                sh 'docker tag ${IMAGE_NAME} ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }

	stage('Docker Push'){
            steps {
              sh 'docker push ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }

        stage('Deploy') {
           steps {
                sh 'pkill node | true'
                sh 'npm install -g forever'
                sh 'forever start src/index.js'
           }
        }
    }
}