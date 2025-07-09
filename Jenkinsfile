pipeline {

  agent  any
  
   triggers {
  githubPush()
}
  environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
  }
  stages {
    stage('Checkout') {
      steps {
       git credentialsId: 'Git-cred', url: 'https://github.com/maheshprince45/star-agile-banking-finance.git'
      }
    }
    stage('Build and Test') {
      steps {
        sh 'ls -ltr'
        // build the project and create a JAR file
        sh 'cd star-agile-banking-finance && mvn clean package'
      }
    }
    stage('Build and Push Docker Image') {
      environment {
        DOCKER_IMAGE = "maheshprince/firstwebapp:${BUILD_NUMBER}"
        // DOCKERFILE_LOCATION = "java-maven-sonar-argocd-helm-k8s/spring-boot-app/Dockerfile"
        REGISTRY_CREDENTIALS = credentials('Docker-cred')
      }
      steps {
        script {
            sh 'cd tar-agile-banking-finance && docker build -t ${DOCKER_IMAGE} .'
            def dockerImage = docker.image("${DOCKER_IMAGE}")
            docker.withRegistry('https://index.docker.io/v1/', "Docker-cred") {
                dockerImage.push()
            }
        }
      }
    }
    
  }
}
