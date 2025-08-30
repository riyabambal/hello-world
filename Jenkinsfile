pipeline {
    agent any
    stages {
        stage('Checkout Code') { // Clones the repository
            steps {
                git 'https://github.com/hello-world/java-app.git'
            }
        }
        stage('Build with Maven') { // Builds the project and creates JAR/WAR
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Run Unit Tests') { // Executes unit tests
            steps {
                sh 'mvn test'
            }
        }
    }
    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}  // <-- Added this closing brace for the 'pipeline' block.
