#! /usr/bin/env groovy

pipeline {
    
    agent {
        label 'maven'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                //sh 'mvn clean install'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                script {
                    openshift.withCluster() {
                        openshift.withProject("rhn-gps-tolarewa-dev") {
                            openshift.newApp('https://github.com/tolarewaju3/codelikethewind.git#jenkinsfile', "--strategy=source").narrow('svc').expose()
            }
          }
        }
            }
        }
    }

}