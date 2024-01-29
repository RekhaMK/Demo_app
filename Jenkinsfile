pipeline {
    agent any
    tools {
      maven 'maven3'
      jdk 'JDK8'
      jfrog 'cli'         
    }
    environment {
        DOCKER_IMAGE_NAME = "slk.jfrog.io/fis-demo-dockerhub/app-image.${BUILD_ID}.${env.BUILD_NUMBER}"
        ARTIFACTORY_ACCESS_TOKEN = credentials('jf_access_token')
        WEBHOOK_URL = credentials("webhook_url")
        BUILD_NAME = "${JOB_NAME}"
        BUILD_NO = "${env.BUILD_NUMBER}"
       
    }
   }
    stages {
        stage ('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/rajesh7979/Demo_app1.git'
            }
        }
        stage ('Build') {
            steps {        
                sh 'mvn clean install'
                sh 'cp ./target/*.jar ./'
                sh 'pwd'
                sh 'docker images'
            }
        }
}
