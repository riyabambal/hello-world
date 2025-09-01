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
                SONARQUBE_SERVER = 'riya-sonar'  // Your Jenkins SonarQube server name
            }
            steps {
                withSonarQubeEnv(SONARQUBE_SERVER) {
                    // Use the specific tool if configured
                    // If you want to specify the scanner manually, create and use the tool
                    // Otherwise, mvn plugin uses sonar-scanner internally
                    sh 'mvn sonar:sonar -Dsonar.projectKey=riya-sonar-project'
                }
            }
        }
    }
    post {
        success {
            echo 'Build and SonarQube analysis completed successfully!'
        }
        failure {
            echo 'There was a failure in build or SonarQube analysis.'
        }
    }
}
