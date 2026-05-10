pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Tool can be used is: Git
                git branch: 'main', url: 'https://github.com/Saipriya753/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Tool: Maven
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Tool: Maven Surefire (mvn test)
                sh 'npm test || true' // Allows pipeline to continue despite test failures
            }
            post {
                always {
                    emailext(
                        to: 'saipriyaa.pyata@gmail.com',
                        subject: "Run Tests stage: ${currentBuild.currentResult}",
                        body: "Run Tests stage completed with status: ${currentBuild.currentResult}. Please check the attached build log.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                // Tool: JaCoCo / Maven coverage plugin
		// Ensure coverage report exists
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit(Security Scan)') {
            steps {
                // Tool: Trivy filesystem scan
                sh 'npm audit || true'// This will show known CVEs in the output
            }
            post {
                always {
                    emailext(
                        to: 'saipriyaa.pyata@gmail.com',
                        subject: "Security Scan stage: ${currentBuild.currentResult}",
                        body: "Security scan completed with status: ${currentBuild.currentResult}. Please check the attached build log.",
                        attachLog: true
                    )
                }
            }
        }
    }
}
