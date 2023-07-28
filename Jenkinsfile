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

String getAuthor(){
    if(params.PROMOTE_ME == true){
        return env.AUTHOR + " The BIG BOSS"
        }
        else {
        return env.AUTHOR
        }
}