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
def installCodeQL() {
      sh 'cd /tmp && test -f /tmp/codeql-runner-linux || curl -O -L  https://github.com/github/codeql-action/releases/latest/download/codeql-runner-linux'
      sh 'chmod a+x /tmp/codeql-runner-linux'
}

def initCodeQL() {
      stage 'Init CodeQL'
      installCodeQL()
      withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'ghe_token')]) {
            sh "/tmp/codeql-runner-linux init --repository Flemh/jenkin-test --github-url https://octodemo.com --github-auth \$ghe_token --languages java,javascript --config-file .github/codeq-config.yml"
      }
}

def build () {
    stage 'Build'
    mvn 'clean install -DskipTests=true -Dmaven.javadoc.skip=true -Dcheckstyle.skip=true -B -V'
}

def mvn(args) {
    withMaven(
        // mavenSettingsConfig: '0e94d6c3-b431-434f-a201-7d7cda7180cb'

        //mavenLocalRepo: '/tmp/m2'
        ) {

      // Run the maven build
          if (codeQl) {
            sh ". codeql-runner/codeql-env.sh && mvn $args -Dmaven.test.failure.ignore -Dmaven.repo.local=/cache"
          } else {
            sh "mvn $args -Dmaven.test.failure.ignore -Dmaven.repo.local=/cache"
          }
     }
}