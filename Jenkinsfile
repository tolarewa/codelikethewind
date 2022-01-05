#! /usr/bin/env groovy

pipeline {
    
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                script {
                    maven {
                        goals('clean')
                        goals('install')
                        properties skipTests: true
                    }
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }

}