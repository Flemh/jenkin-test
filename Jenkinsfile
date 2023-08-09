pipeline {
    agent any

    node {
        stage('Build') {
             withCodeQL(codeql: 'CodeQL 2.5.5') {
                sh 'codeql --version'
            }
        }
    }
           /*  agent {
                dockerContainer {
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
        }*/
    //}
}