#! /usr/bin/env groovy

pipeline {
    
    agent {
        label 'maven'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                maven {
                        goals('clean')
                        goals('install')
                        properties skipTests: true
                    }
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
            }
        }
    }

}