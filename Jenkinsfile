pipeline {
    agent any

    stages {
        stage('Checkout') {
            agent any
            steps {
                checkout scm
            }
        }
        stage('Build') {
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v $HOME/.m2:/root/.m2'
                }
            }
            steps {
                echo 'Building..'
                sh 'mvn -f ./SmallestSpringApp/pom.xml clean package'
                stash includes: 'SmallestSpringApp/target/*', name: 'app'
            }
        }
        stage('Deploy') {
            agent any
            steps {
                unstash 'app'
                // sh 'cd SmallestSpringApp/target'
                sh 'ls'
                sh 'docker build -t smallest-spring-app:${BUILD_NUMBER} .'
                // sh 'docker tag -t smallest-spring-app:${BUILD_NUMBER} smallest-spring-app:latest'
                sh 'docker images'
            }
        }
    }
}
