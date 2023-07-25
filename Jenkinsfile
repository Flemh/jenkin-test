pipeline {
    agent any

    environment {
    DEMO = '1.3'
    AUTHOR = 'Przemek'
    }

    stages {
        stage('stage-1') {
            steps{
                echo "this is build number $BUILD_NUMBER of demo $DEMO by $AUTHOR"
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