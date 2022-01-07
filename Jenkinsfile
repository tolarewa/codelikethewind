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
    stage('Create Image') {
      steps {
        echo 'Testing..'
        
        script {
          openshift.withCluster() {
            openshift.withProject("rhn-gps-tolarewa-dev") {
                openshift.newBuild("--name=codelikethewind", "--docker-image=registry.redhat.io/jboss-eap-7/eap74-openjdk8-openshift-rhel7", "--binary")

                openshift.selector("bc", "codelikethewind").startBuild("--from-file=target/simple-servlet-0.0.1-SNAPSHOT.war", "--wait")

            }

          }
        }
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploying....'
        script {
          openshift.withCluster() {
            openshift.withProject("rhn-gps-tolarewa-dev") {

              openshift.newApp("codelikethewind:latest", "--name=codelikethewind").narrow('svc').expose()

            }

          }
        }
      }
    }
  }
}