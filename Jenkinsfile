pipeline {
 
    agent any
 
 
    environment {
 
        MAVEN_HOME = '/usr/share/maven' // Path to Maven installation
 
 
    }
 
 
    stages {
 
        stage('Checkout Code') { // Clones the repository
 
            steps {
 
                git 'https://github.com/riyabambal/hello-world.git'
 
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
 
}
