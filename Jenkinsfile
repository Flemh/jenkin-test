
    node {
        stage('Build') {
             withCodeQL(codeql: 'CodeQL 2.14.2') {
                sh 'codeql --version'
            }
        }
    }
