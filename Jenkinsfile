pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/riyabambal/hello-world.git'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Run Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Code Analysis with SonarQube') {
            environment {
                SONARQUBE_SERVER = 'sonarqube' // Name of the SonarQube server configured in Jenkins
            }
            steps {
                withSonarQubeEnv(SONARQUBE_SERVER) {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }
    post {
        success {
            echo 'Build and Sonar analysis successful!'
        }
        failure {
            echo 'Build or Sonar analysis failed!'
        }
    }
}
