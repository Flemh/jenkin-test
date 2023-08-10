pipeline {
    agent {
        docker { image 'maven:3.9.3-eclipse-temurin-17' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}