pipeline {
    agent any

    stages {
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
        stage('Build image') {
            agent any
            steps {
                unstash 'app'
                // sh 'cd SmallestSpringApp/target'
                sh 'ls'
                sh 'docker build -t mk0dockerhub/smallest-spring-app:${BUILD_NUMBER} .'
                sh 'docker tag mk0dockerhub/smallest-spring-app:${BUILD_NUMBER} mk0dockerhub/smallest-spring-app:latest'
            }
        }
        stage('CD') {
            agent any
            steps {
                withDockerRegistry([ credentialsId: "docker-hub", url: "https://index.docker.io/v1/" ]) {
                    sh 'docker push mk0dockerhub/smallest-spring-app:latest'
                }
            }
        }
    }
}
