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
        stage('shell') {
                    steps{
                        sh """
                           chmod +x mvnw && ./mvnw clean package
                            """
                            // JUnit Results
                            		junit 'target/surefire-reports/*.xml'
                            		archiveArtifacts allowEmptyArchive: false, artifacts: 'target/*.jar', caseSensitive: true, defaultExcludes: true, fingerprint: false, onlyIfSuccessful: false
                    }
        }
    }
}