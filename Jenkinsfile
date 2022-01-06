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

              def app = openshift.selector("all", ['app': 'codelikethewind'])

              if(app.exists()){
                def bc = openshift.selector( "bc", "codelikethewind")
                bc.startBuild()
              }
              else{
                openshift.newApp('registry.redhat.io/jboss-eap-7/eap74-openjdk8-openshift-rhel7~https://github.com/tolarewaju3/codelikethewind.git#jenkinsfile', "--strategy=source").narrow('svc').expose()
              }

            }

          }
        }
      }
    }
  }
}