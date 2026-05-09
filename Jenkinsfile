pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Tool: Git
                git branch: 'main', url: 'https://github.com/Saipriya753/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Tool: Maven
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Run Tests') {
            steps {
                // Tool: Maven Surefire (mvn test)
                sh 'mvn test || true'
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
                sh 'mvn test jacoco:report || true'
            }
        }

        stage('Security Scan') {
            steps {
                // Tool: Trivy filesystem scan
                sh 'trivy fs . || true'
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
