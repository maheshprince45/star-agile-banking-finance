pipeline {

  agent  any

  environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
  }
  stages {
    stage('Checkout') {
      steps {
        git credentialsId: 'Git-cred', 
            url: 'https://github.com/maheshprince45/star-agile-banking-finance.git', 
            branch: 'master'
      }
    }
    stage('Build and Test') {
      steps {
        sh 'ls -ll'
        // build the project and create a JAR file
        sh 'mvn clean package -DskipTests'
      }
    }
    stage('Build and Push Docker Image') {
      environment {
        DOCKER_IMAGE = "maheshprince/firstwebapp:rolling"
        REGISTRY_CREDENTIALS = credentials('Docker-cred')
      }
      steps {
        script {
            sh 'docker build -t ${DOCKER_IMAGE} .'
            sh "echo ${REGISTRY_CREDENTIALS_PSW} | docker login -u ${REGISTRY_CREDENTIALS_USR} --password-stdin"
            sh "docker push ${DOCKER_IMAGE}"
        }
        }
      }
    }
    
  }

