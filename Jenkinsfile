pipeline {
    agent { label 'maven_jdk8'}
    triggers { pollSCM ('* * * * *') }
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
                sh "mkdir -p /tmp/${JOB_NAME}/${BUILD_ID} && cp ./gameoflife-web/target/gameoflife.war /tmp/${JOB_NAME}/${BUILD_ID}/" 
            }
        }
        stage('archeive artifcat and publish junit test') {
            steps {
                archiveArtifacts artifacts: '**/target/gameoflife.war',
                         allowEmptyArchive: false,
                         onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml',
                        allowEmptyResults: true
                                     
            }
        }
    }
    post {
        success {
            mail subject: "Jenkins Build with ${JOB_NAME} with id ${BUILD_ID} is success",
                body: "use this URL ${BUILD_URL} for more info",
                to: "team-all-sdcorp@sdcorp.net",
                from: "devops@sdcorp.net"
            }
        failure {
            mail subject: "Jenkins Build with ${JOB_NAME} with id ${BUILD_ID} is success",
                body: "use this URL ${BUILD_URL for more info}",
                to: "${GIT_AUTHOR_EMAIL}",
                from: "devops@sdcorp.net"
        }    
    }
}