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

        stage('OWASP Dependency-Check') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    sh """
                        ${OWASP_TOOL_HOME}/bin/dependency-check.sh \
                        --project "${SONAR_PROJECT_KEY}" \
                        --scan . \
                        --format XML \
                        --format HTML \
                        --out reports/dependency-check
                    """
                    dependencyCheckPublisher(
                        pattern: 'reports/dependency-check/dependency-check-report.xml',
                        stopBuild: false
                    )
                }
            }
        }

        stage('SonarQube Analysis') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner -Dsonar.projectKey=${SONAR_PROJECT_KEY} -Dsonar.sources=src -Dsonar.login=${SONAR_TOKEN}'
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Trivy Container Scan') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                script {
                    def tag = "${DOCKER_REPO}:1.0.${env.BUILD_NUMBER}"
                    sh """
                        trivy image \
                        --severity HIGH,CRITICAL \
                        --exit-code 1 \
                        --no-progress \
                        ${tag} > reports/trivy-report.txt
                    """
                }
            }
            post {
                always {
                    archiveArtifacts artifacts: 'reports/trivy-report.txt', fingerprint: true
                }
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
