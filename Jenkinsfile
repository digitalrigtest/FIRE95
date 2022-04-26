pipeline {
    agent any
    stages {
        stage('git checkout') {
            steps {
            sh "echo git checkout"
            }
        }
        stage('sonar analysis') {
            environment {
                scannerHome = tool 'SonarQube'
            }
            steps{
                withSonarQubeEnv('rigSonarQubeServer') {
                    sh '${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=SONAR_KEY -Dsonar.exclusions=src/test/java/**'
                }
            }
        }
        stage('maven publish to nexus') {
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'TomcatMavenApp', classifier: '', file: 'target/TomcatMavenApp-2.0.war', type: 'war']], credentialsId: 'MY_NEXUS_CRED_ID', groupId: 'com.sarav', nexusUrl: 'nexus-app.wiprodigitalrig.com/', nexusVersion: 'nexus3', protocol: 'https', repository: 'TomcatMavenApp/', version: '2.0'
            }
        }
    }
}
