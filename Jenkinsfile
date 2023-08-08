pipeline {
    agent any


    parameters {
        booleanParam(
            defaultValue: false,
            description: 'Do You want to promote Przemek',
            name: 'PROMOTE_ME'
        )
    }


    environment {
    DEMO = '1.3'
    AUTHOR = 'Przemek'
    }

    stages {
        stage('stage-1') {
        environment {
            AUTHOR_AFTER = getAuthor()
            }
            steps{
                echo "this is build number $BUILD_NUMBER of demo $DEMO by $AUTHOR_AFTER"
            }
        }
        stage('install-codeQl') {
                steps{
                    installCodeQL()
                    }
                }
        stage('build') {
                    steps{
                        sh 'chmod +x build.sh'
                        sh """
                           ./build.sh
                            """
                    }
        }

       stage('test') {
                     steps{
                         // JUnit Results
                        	junit 'target/surefire-reports/*.xml'
                     }
               }
    }
    post {
        success {
            archiveArtifacts allowEmptyArchive: false, artifacts: 'target/*.jar', caseSensitive: true, defaultExcludes: true, fingerprint: false, onlyIfSuccessful: false
        }
    }
}

def installCodeQL() {
      sh 'cd /tmp && test -f /tmp/codeql-runner-linux || curl -O -L  https://github.com/github/codeql-action/releases/latest/download/codeql-runner-linux'
      sh 'chmod a+x /tmp/codeql-runner-linux'
}

String getAuthor(){
    if(params.PROMOTE_ME == true){
        return env.AUTHOR + " The BIG BOSS"
        }
        else {
        return env.AUTHOR
        }
}