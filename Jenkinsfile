pipeline {
    agent { label 'maven_jdk8' }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/dhanurkarcorp/game-of-life.git',
                    branch: 'declarative'
            }        
        }
        stage('build') {
            steps {
                sh 'mvn package'
            }    
        }
        stage('archive artifact') {
            steps {
                archiveArtifacts artifacts: '**/target/gameoflife.war',
                allowEmptyArchive: false,
                onlyIfSuccessful: true
            }    
        }
        stage('publish test result') {
            steps {
                junit testResults: '**/surefire-reports/TEST-*.xml',
                allowEmptyResults: true
            }    
        }
    }    
}