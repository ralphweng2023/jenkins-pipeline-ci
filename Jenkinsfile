pipeline {
    agent any

    environment {
        // Defining environment variables here
        STAGING_SERVER = 'AWS_EC2_staging.example.com'
        PRODUCTION_SERVER = 's214331778.com'
        RECIPIENT_EMAIL = 's214331778@deakin.edu.au'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                // The command to build project, e.g., 'mvn clean package'
                sh 'echo "Maven: mvn clean package"'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running tests...'
                // The command to run tests, e.g., 'mvn test'
                sh 'echo "Testing tools: JUnit for unit tests, Mockito for integration tests"'
            }
            post {
                always {
                    // Send email notification after tests (part of tasksheet)
                    emailext(
                        subject: "Jenkins Build #${BUILD_NUMBER} - Tests: ${currentBuild.currentResult}",
                        body: "Please see the attached logs for more details.",
                        to: "${RECIPIENT_EMAIL}",
                        attachLog: true
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Analyzing code...'
                // The command to analyze  code, e.g., SonarQube analysis
                sh 'echo "Code Analysis tool: SonarQube"'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                // The command to perform security scan, e.g., using OWASP Dependency Check
                sh 'echo "Security Scan tool: OWASP Dependency Check"'
            }
            post {
                always {
                    // Send email notification after security scan (part of tasksheet)
                    emailext(
                        subject: "Jenkins Build #${BUILD_NUMBER} - Security Scan: ${currentBuild.currentResult}",
                        body: "Please see the attached logs for more details.",
                        to: "${RECIPIENT_EMAIL}",
                        attachLog: true
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                // Simulate a deployment to a staging server
                sh "echo 'Deploying to ${STAGING_SERVER}'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Simulate running integration tests in the staging environment
                sh 'echo "Running integration tests on staging server"'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment...'
                // Simulate a deployment to a production server
                sh "echo 'Deploying to ${PRODUCTION_SERVER}'"
            }
        }
    }

    post {
        failure {
            // Send email notification for any failed stage in the pipeline
            emailext(
                subject: "Jenkins Build #${BUILD_NUMBER} - Failed",
                body: "Unfortunately, the build has failed. Please check the Jenkins logs for more information.",
                to: "${RECIPIENT_EMAIL}",
                attachLog: true
            )
        }
    }
}
