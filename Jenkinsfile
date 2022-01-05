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
                        openshift.withProject("test-project") {
                            openshift.newApp('registry.access.redhat.com/redhat-openjdk-18/openjdk18-openshift:1.6~https://github.com/remkohdev/spring-client', "--name=springclient", "--strategy=source", "--allow-missing-images", "--build-env=\'JAVA_APP_JAR=hello.jar\'").narrow('svc').expose()
            }
          }
        }
            }
        }
    }

}