pipeline {
    agent any
    environment{
        DOCKER_TAG = ""
    }
    stages {
        stage('Code Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Manash2712/java-app-jenkins'
                script{
                    env.DOCKER_TAG = getShortCommitHash()
                }
            }
        }
        stage('Sonarqube Scanner') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh "mvn sonar:sonar"
                }
            }
        }
        stage('Quality Gate'){
            steps{
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage("Docker Build"){
            steps{
                sh "docker build -t manashchauhan/java-app-jenkins:{env.DOCKER_TAG} ."
            }
        }
    }
}
def getShortCommitHash(){
    return sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
}
