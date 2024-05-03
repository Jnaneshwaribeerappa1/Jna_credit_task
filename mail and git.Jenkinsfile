pipeline {
    agent any
    
    environment {
        // Define your environment variables and tools configurations here
    }

    triggers {
        // This triggers the build on every git push
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Example: Maven build tool
                sh 'mvn clean package'
            }
            post {
                success {
                    emailext(
                        recipients: 'jnaneshwarib63@gmail.com',
                        subject: "Build Success - ${env.JOB_NAME}",
                        body: "The Build was successful. See details at ${env.BUILD_URL}."
                    )
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                // Example: Using JUnit for Java projects
                sh 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Analyzing the code...'
                // Example: SonarQube for code quality analysis
                sh 'sonar-scanner'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                // Example: Using OWASP ZAP for security scans
                sh 'zap-scan.sh'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                // Example: Deploy to AWS EC2 using scripts or AWS CLI
                sh 'deploy-staging.sh'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Example: Using Selenium for integration tests
                sh 'selenium-tests.sh'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
                // Example: Deploy to AWS EC2 using scripts or AWS CLI
                sh 'deploy-production.sh'
            }
        }
    }

    post {
        always {
            // Send email notifications at the end of each stage
            emailext (
                recipients: 'your-email@example.com',
                subject: "Jenkins Pipeline Status - ${currentBuild.fullDisplayName}",
                body: """<p>Stage '${env.STAGE_NAME}' completed.</p>
                         <p>Status: ${currentBuild.currentResult}</p>
                         <p>See full details at: <a href='${env.BUILD_URL}'>Build Log</a></p>""",
                attachLog: true
            )
        }
    }
}
