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
                sh "./mvnw clean package -DskipUnitTests=true"
            }

            stage('Unit-Tests') {
                sh "./mvnw test"
            }

            stage('Integration-Tests') {
                // TODO
            }

            stage('Deploy') {
                if (git.isProductionBranch()) {
                    sh "GPG_TTY=\$(tty) ./mvnw -Prelease package source:jar gpg:sign install:install deploy:deploy"
                } else {
                    sh "./mvnw deploy"
                }

                stage('Sonar') {
                    withSonarQubeEnv('sonarcloud.io') {
                        sh "./mvnw clean org.jacoco:jacoco-maven-plugin:prepare-agent package sonar:sonar -Dsonar.host.url=https://sonarcloud.io -Dsonar.organization=hypery2k-github"
                    }
                }
            }

            archiveArtifacts artifacts: '*/target/*.jar,*/target/*.war'
            junit healthScaleFactor: 1.0, testResults: '*/target/surefire-reports/TEST*.xml'
        } catch (e) {
            mail subject: "${env.JOB_NAME} (${env.BUILD_NUMBER}): Error on build", to: 'github@martinreinhardt-online.de', body: "Please go to ${env.BUILD_URL}."
            throw e
        }
    }
}
