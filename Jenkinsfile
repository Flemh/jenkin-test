pipeline{
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
}