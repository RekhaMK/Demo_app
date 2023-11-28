pipeline {
    agent any
    environment {
        GIT_REPO_NAME = 'Demo_app'
        GITHUB_BRANCH = 'main'
        GIT_USER_NAME = 'rajesh7979'
        PATH = "$PATH:/usr/share/maven"
    }
    stages {
        stage ('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/rajesh7979/Demo_app.git'
            }
        }
        stage ('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Upload'){
            steps{
                rtUpload (
                 serverId:"jfrog-artifactory",
                  spec: '''{
                   "files": [
                      {
                      "pattern": "*.war",
                      "target": "fis-demo-release"
                      }
                            ]
                           }''',
                        )
            }
        }
        stage ('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarqube-demo') {
                sh 'mvn sonar:sonar'
                }
            }
        }   
        
        stage ('Build docker image') {
            steps {
                script {
                    sh 'sudo docker build -t fis-demo/app-image.${BUILD_ID}  .'
                    sh 'sudo docker images'
                }
            }
        }
        stage('Push artifacts into artifactory') {
            steps {
              rtUpload (
                serverId: 'jfrog-artifactory',
                spec: '''{
                      "files": [
                        {
                          "pattern": "*.app-image.${BUILD_ID}",
                           "target": "fis-demo-release"
                        }
                    ]
                }'''
              )
            }
        }
    }
    //Job Status check
    post {
        success {
            echo 'Build successfully!'
        }

        failure {
            echo 'Build Failed'
        }
    }
}

    

    
