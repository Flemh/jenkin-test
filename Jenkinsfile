pipeline {
    agent any
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3.9.3-eclipse-temurin-17-alpine'
                    // Run the container on the node specified at the
                    // top-level of the Pipeline, in the same workspace,
                    // rather than on a new node entirely:
                    args '-u root:root'
                    reuseNode true
                }
            }
            steps {
                sh 'chmod -R 755 /var/jenkins_home/workspace'
                sh 'mvn --version'

            }
        }
    }
}