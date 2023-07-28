pipeline {
    agent any

    parameters {
        booleanParameter(
            defaultValue: false,
            description: 'Do You want to promote Przemek',
            name: 'PROMOTE_ME'
        )
    }


    environment {
    DEMO = '1.3'
    AUTHOR = 'Przemek'
    AUTHOR_AFTER = getAuthor()
    }

    stages {
        stage('stage-1') {
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
if(PROMOTE_ME == true){
return AUTHOR+ " The BIG BOSS";}
else return AUTHOR;
}