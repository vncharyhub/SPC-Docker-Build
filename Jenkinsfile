pipeline {
    agent {
        label 'spc_java_11'
    }
    triggers {
        pollSCM '0 */1 * * *'
    }
    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/vncharyhub/SPC-Docker-Build.git'
            }
        }
        stage('Docker-Build') {
            steps {
              sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
              sh 'docker image tag $JOB_NAME:v1.$BUILD_ID itschary/$JOB_NAME:v1.$BUILD_ID'
              sh 'docker image tag $JOB_NAME:v1.$BUILD_ID itschary/$JOB_NAME:latest'
            }
        }
        stage('Push Image to Docker-Registry') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Docker-hub', passwordVariable: 'dockerhubpassword', usernameVariable: 'itschary')]) {
                    sh 'docker login -u itschary -p ${dockerhubpassword}'
                    sh 'docker image push itschary/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image push itschary/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image rmi $JOB_NAME:v1.$BUILD_ID itschary/$JOB_NAME:v1.$BUILD_ID itschary/$JOB_NAME:latest'
                }
            }
        }
    }
}
