pipeline {
    agent any
    stages {
        stage('Build') {
            agent {
                docker {
                sh 'chmod +x /var/jenkins_home/workspace/demo2'
                    image 'maven:3.9.3-eclipse-temurin-17-alpine'
                    // Run the container on the node specified at the
                    // top-level of the Pipeline, in the same workspace,
                    // rather than on a new node entirely:
                    reuseNode true
                }
            }
            steps {
                sh 'mvn --version'
            }
        }
    }
}