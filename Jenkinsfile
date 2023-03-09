pipeline {
    agent { label 'maven_jdk8'}
    triggers { pollSCM ('H/30 * * * *') }
    parameters {
        string(name: 'MAVEN_GOAL', defaultValue: 'package', description: 'MAVEN GOAL') }
    stages {
        stage('vcs') {
            steps {
            git url : 'https://github.com/dhanurkarcorp/game-of-life.git',
                branch: 'master'
            }
        }
        stage('package') {
            environment {
                PATH="/usr/lib/jvm/java-1.8.0-openjdk-amd64/bin:$PATH"
            }
            steps {
                sh "mvn ${params.MAVEN_GOAL}"
                sh "yum install"
            }
        }
        stage('archeive artifcat and publish junit test') {
            steps {
                archiveArtifacts artifacts: '**/target/gameoflife.war',
                         allowEmptyArchive: false,
                         onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml',
                        allowEmptyResults: true
                mail subject: 'build failed'
                        to: 'surajd@sdcorp.net'                     
            }
        }
    }
}