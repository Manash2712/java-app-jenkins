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
        /*
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
        */
        stage("Docker Build"){
            steps{
                sh "docker build -t manashchauhan/java-app-jenkins:${env.DOCKER_TAG} ."
            }
        }
        stage("Trivy Scan"){
            steps{
                sh "export TMPDIR=${WORKSPACE} && trivy image --severity HIGH,CRITICAL --exit-code 0 manashchauhan/java-app-jenkins:${env.DOCKER_TAG}"
            }
        }
        stage("Push Docker Image"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerHubToken', passwordVariable: 'dockerhubPW', usernameVariable: 'dockerhubUser')]) {
                    sh "docker login -u ${dockerhubUser} -p ${dockerhubPW}"
                    sh "docker push manashchauhan/java-app-jenkins:${env.DOCKER_TAG}"
                }
            }
        }
    }
}
def getShortCommitHash(){
    return sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
}
