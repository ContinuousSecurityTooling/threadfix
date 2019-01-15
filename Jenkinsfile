properties properties: [
        [$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '30', numToKeepStr: '10']],
        disableConcurrentBuilds()
]

@Library('mare-build-library')
def git = new de.mare.ci.jenkins.Git()

timeout(60) {
    node {
        def buildNumber = env.BUILD_NUMBER
        def branchName = env.BRANCH_NAME
        def workspace = env.WORKSPACE
        def buildUrl = env.BUILD_URL

        try {

            // PRINT ENVIRONMENT TO JOB
            echo "workspace directory is $workspace"
            echo "build URL is $buildUrl"
            echo "build Number is $buildNumber"
            echo "branch name is $branchName"
            echo "PATH is $env.PATH"

            stage('Checkout') {
                checkout scm
            }
            stage('Build') {
                sh "./mvnw install:install-file -Dfile=lib/com/microsoft/tfs/sdk/com.microsoft.tfs.sdk/11.0.0/com.microsoft.tfs.sdk-11.0.0.jar -DgroupId=com.microsoft.tfs.sdk -DartifactId=com.microsoft.tfs.sdk -Dversion=11.0.0 -Dpackaging=jar"
                sh "./mvnw install:install-file -Dfile=threadfix-scanner-plugin/zaproxy/lib/com/owasp/zap/zap/2.2.2/zap-2.2.2.jar -DgroupId=com.owasp.zap -DartifactId=zap -Dversion=2.2.2 -Dpackaging=jar"
                sh "./mvnw install:install-file -Dfile=threadfix-scanner-plugin/zaproxy/lib/com/owasp/zap/zaphelp/2.2.2/zaphelp-2.2.2.jar -DgroupId=com.owasp.zap -DartifactId=zaphelp -Dversion=2.2.2 -Dpackaging=jar"
                sh "./mvnw install:install-file -Dfile=threadfix-scanner-plugin/zaproxy/lib/com/owasp/zap/xom/1.2.6/xom-1.2.6.jar -DgroupId=com.owasp.zap -DartifactId=xom -Dversion=1.2.6 -Dpackaging=jar"
                sh "./mvnw install:install-file -Dfile=threadfix-scanner-plugin/zaproxy/lib/com/owasp/zap/java-getopt/1.0.13/java-getopt-1.0.13.jar -DgroupId=com.owasp.zap -DartifactId=java-getopt -Dversion=1.0.13 -Dpackaging=jar"
                sh "./mvnw install:install-file -Dfile=threadfix-scanner-plugin/zaproxy/lib/com/owasp/zap/jgoodies-looks/2.4.0/jgoodies-looks-2.4.0.jar -DgroupId=com.owasp.zap -DartifactId=jgoodies-looks -Dversion=2.4.0 -Dpackaging=jar"
                sh "./mvnw install:install-file -Dfile=threadfix-scanner-plugin/zaproxy/lib/com/owasp/zap/lablib-checkboxtree/3.2/lablib-checkboxtree-3.2.jar -DgroupId=com.owasp.zap -DartifactId=lablib-checkboxtree -Dversion=3.2 -Dpackaging=jar"
                sh "./mvnw clean install -DskipTests=true -q"
                sh "./mvnw clean package -DskipTests=true -B -q"
                // image = docker.build('continuoussecuritytooling/threadfix')
            }

            stage('Unit-Tests') {
                // FIXME sh "./mvnw test"
            }

            stage('Integration-Tests') {
                // TODO
            }

            stage('Deploy') {
                /* TODO
                docker.withRegistry('https://registry-1.docker.io/v2/', 'docker-hub-continuoussecuritytooling') {
                    image.push("${buildNumber}")
                }
                */

                /* TODO
                if (git.isProductionBranch()) {
                    sh "GPG_TTY=\$(tty) ./mvnw -Prelease package source:jar gpg:sign install:install deploy:deploy"
                } else {
                    sh "./mvnw deploy"
                }
                */
            }

            stage('Sonar') {
                withSonarQubeEnv('sonarcloud.io') {
                    sh "./mvnw clean org.jacoco:jacoco-maven-plugin:prepare-agent package sonar:sonar -Dsonar.host.url=https://sonarcloud.io -Dsonar.organization=hypery2k-github"
                }
            }

            archiveArtifacts artifacts: '*/target/*.jar,*/target/*.war'
            // junit healthScaleFactor: 1.0, testResults: '*/target/surefire-reports/TEST*.xml'
        } catch (e) {
            mail subject: "${env.JOB_NAME} (${env.BUILD_NUMBER}): Error on build", to: 'github@martinreinhardt-online.de', body: "Please go to ${env.BUILD_URL}."
            throw e
        }
    }
}
