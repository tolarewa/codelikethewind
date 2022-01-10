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
    stage('Create Container Image') {
      steps {
        echo 'Create Container Image..'
        
        script {
          openshift.withCluster() {
            openshift.withProject("rhn-gps-tolarewa-dev") {
                def buildConfigExists = openshift.selector("bc", "codelikethewind").exists()

                if(!buildConfigExists){
                    openshift.newBuild("--name=codelikethewind", "--docker-image=registry.redhat.io/jboss-eap-7/eap74-openjdk8-openshift-rhel7", "--binary")
                }

                openshift.selector("bc", "codelikethewind").startBuild("--from-file=target/simple-servlet-0.0.1-SNAPSHOT.war", "--follow")

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

              def deploymentExists = openshift.selector("dc", "codelikethewind").exists()

              if(!deploymentExists){
                echo 'Deployment doesnt exists'
                openshift.newApp('codelikethewind').narrow('svc').expose()
              }
              else{
                echo 'Deployment exists'
              }

            }

          }
        }
      }
    }
  }
}