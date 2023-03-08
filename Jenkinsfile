node('maven_jdk8') {
    stage('version control') {
        git url: 'https://github.com/dhanurkarcorp/game-of-life.git',
            branch: 'scripted'
    }
    stage('build the code') {
        sh 'export PATH="/usr/lib/jvm/java-1.8.0-openjdk-amd64/bin:$PATH" && mvn package'
    }
    stage('archive the artifact') {
        archiveArtifacts artifacts: '**/target/gameoflife.war',
                        onlyIfSuccessful: true
    }
    stage('publish junit test') {
        junit testResults: '**/surefire-reports/TEST-*.xml',
                    allowEmptyResults: true
    }
}