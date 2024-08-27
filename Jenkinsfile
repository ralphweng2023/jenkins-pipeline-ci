pipeline {
    agent {
        node {
            label 'master'  // This will select the master node
        }
    }

    environment {
        // Defining environment variables here
        STAGING_SERVER = 'AWS_EC2_staging.example.com'
        PRODUCTION_SERVER = 's214331778.com'
        RECIPIENT_EMAIL = 's214331778@deakin.edu.au'
    }

    stages {
        stage('Mark Directory as Safe for Git') {
            steps {
                echo 'Marking workspace directory as safe for Git...'
                sh 'git config --global --add safe.directory /var/jenkins_home/workspace/my-project'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Replace with the actual build command, e.g., 'mvn clean package' for Java or 'npm run build' for Node.js
                sh 'echo "Maven: mvn clean package"'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running tests...'
                // Replace with the actual test command, e.g., 'mvn test' for Java or 'npm test' for Node.js
                sh 'echo "Testing tools: JUnit for unit tests, Mockito for integration tests"'
            }
            post {
                always {
                    // Send email notification after tests
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
                // Replace with the actual code analysis tool command, e.g., SonarQube
                sh 'echo "Code Analysis tool: SonarQube"'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                // Replace with the actual security scan tool command, e.g., using OWASP Dependency Check
                sh 'echo "Security Scan tool: OWASP Dependency Check"'
            }
            post {
                always {
                    // Send email notification after security scan
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
                echo "Deploying to staging environment: ${STAGING_SERVER}..."
                // Simulate a deployment to a staging server, replace with actual deployment command
                sh "echo 'Deploying to ${STAGING_SERVER}'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Simulate running integration tests in the staging environment, replace with actual integration test command
                sh 'echo "Running integration tests on staging server"'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Deploying to production environment: ${PRODUCTION_SERVER}..."
                // Simulate a deployment to a production server, replace with actual deployment command
                sh "echo 'Deploying to ${PRODUCTION_SERVER}'"
            }
        }
    }

    post {
        success {
            emailext(
                subject: "Jenkins Build #${BUILD_NUMBER} - Success",
                body: "The pipeline has successfully completed. Please review the logs if necessary.",
                to: "${RECIPIENT_EMAIL}",
                attachLog: false
            )
        }
        failure {
            emailext(
                subject: "Jenkins Build #${BUILD_NUMBER} - Failed",
                body: "Unfortunately, the build has failed. Please check the attached logs for more information.",
                to: "${RECIPIENT_EMAIL}",
                attachLog: true
            )
        }
        always {
            echo 'Cleaning up workspace...'
            cleanWs()  // Clean up the workspace after the build
        }
    }
}
