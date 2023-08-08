import groovy.json.JsonOutput
import groovy.json.JsonSlurper




def label = "mypod-${UUID.randomUUID().toString()}"
podTemplate(label: label, yaml: """
spec:
      containers:
      - name: mvn
        image: maven:3.3.9-jdk-8
        command:
        - cat
        tty: true
        volumeMounts:
        - mountPath: /cache
          name: maven-cache
      volumes:
      - name: maven-cache
        hostPath:
          # directory location on host
          path: /tmp
          type: Directory
""",
{
    node (label) {
      container ('mvn') {
        // env.JAVA_HOME="${tool 'Java SE DK 8u131'}"
        // env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"

        server = Artifactory.server "artifactory"
        buildInfo = Artifactory.newBuildInfo()
        buildInfo.env.capture = true

        (ref, commit) = checkout()
        // pull request or feature branch

                // PR-build
                codeQl = true
                initCodeQL()
                build()
                //analyzeAndUploadCodeQLResults(ref, commit)
                unitTest()
                // sonar()

    }
 }
})

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
                    withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'ghe_token')]) {
                                sh "/tmp/codeql-runner-linux init --repository Flemh/jenkin-test --github-url https://github.com --github-auth \$ghe_token --languages java,javascript --config-file .github/codeq-config.yml"
                          }
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
      sh 'mkdir ./tmp'
      sh 'cd /tmp && test -f /tmp/codeql-runner-linux || curl -O -L  https://github.com/github/codeql-action/releases/latest/download/codeql-runner-linux'
      sh 'chmod a+x /tmp/codeql-runner-linux'
}

def getRepoSlug() {
    tokens = "${env.JOB_NAME}".tokenize('/')
    org = tokens[tokens.size()-3]
    repo = tokens[tokens.size()-2]
    return "${org}/${repo}"
}

def checkout () {
    stage 'Checkout code'
    context="continuous-integration/jenkins/"
    context += isPRMergeBuild()?"pr-merge/checkout":"branch/checkout"
    def scmVars = checkout scm
    setBuildStatus ("${context}", 'Checking out completed', 'SUCCESS')
    if (isPRMergeBuild()) {
      prMergeRef = "refs/pull/${getPRNumber()}/merge"
      mergeCommit=sh(returnStdout: true, script: "git show-ref ${prMergeRef} | cut -f 1 -d' '")
      echo "Merge commit: ${mergeCommit}"
      return [prMergeRef, mergeCommit]
    } else {
      return ["refs/heads/${env.BRANCH_NAME}", scmVars.GIT_COMMIT]
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