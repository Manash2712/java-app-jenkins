pipeline {
    agent any

    stages {
        stage('Code Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Manash2712/java-app-jenkins'
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
    }
}
