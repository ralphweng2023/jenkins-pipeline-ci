pipeline {
    agent {
        node {
            customWorkspace '/var/jenkins_home/workspace/my-project'  // Setting custom workspace
        }
    }

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
                // Replace this with your actual build command, for example:
                sh 'mvn clean package'  // For Maven projects
                // For Node.js projects, use:
                // sh 'npm run build'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                // Replace this with your actual test command, for example:
                sh 'mvn test'  // For Java/Maven projects
                // For Node.js projects, use:
                // sh 'npm test'
            }
            post {
                always {
                    emailext(
                        subject: "Jenkins Build #${BUILD_NUMBER} - Tests: ${currentBuild.currentResult}",
                        body: "The testing stage has completed with result: ${currentBuild.currentResult}. Please review the attached logs for more details.",
                        to: "${RECIPIENT_EMAIL}",
                        attachLog: true
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Analyzing code...'
                // Replace this with your actual code analysis tool command, for example:
                sh 'sonar-scanner'  // For SonarQube analysis
                // Or for Node.js linting:
                // sh 'npm run lint'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                // Replace this with your actual security scan tool command, for example:
                sh 'dependency-check --project myApp --scan ./'
                // Or for Node.js security audit:
                // sh 'npm audit'
            }
            post {
                always {
                    emailext(
                        subject: "Jenkins Build #${BUILD_NUMBER} - Security Scan: ${currentBuild.currentResult}",
                        body: "The security scan stage has completed with result: ${currentBuild.currentResult}. Please review the attached logs for more details.",
                        to: "${RECIPIENT_EMAIL}",
                        attachLog: true
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo "Deploying to staging environment: ${STAGING_SERVER}..."
                // Replace this with your actual deployment command, for example:
                sh "scp -r ./target/my-app.war ec2-user@${STAGING_SERVER}:/var/www/my-app"  // For Java/Maven projects
                // For Node.js projects, you might use:
                // sh "scp -r ./build/* ec2-user@${STAGING_SERVER}:/var/www/my-app"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on the staging environment...'
                // Replace this with your actual integration test command, for example:
                sh 'mvn verify'  // For Java/Maven projects
                // For Node.js projects, you might use:
                // sh 'npm run test:integration'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Deploying to production environment: ${PRODUCTION_SERVER}..."
                // Replace this with your actual deployment command, for example:
                sh "scp -r ./target/my-app.war ec2-user@${PRODUCTION_SERVER}:/var/www/my-app"  // For Java/Maven projects
                // For Node.js projects, you might use:
                // sh "scp -r ./build/* ec2-user@${PRODUCTION_SERVER}:/var/www/my-app"
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
