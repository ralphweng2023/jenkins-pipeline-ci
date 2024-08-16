pipeline {
    agent any

    environment {
        NODE_HOME = tool name: 'NodeJS 14', type: 'NodeJS'
        PATH = "${NODE_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the code from GitHub...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the code...'
                // Optional: Compile or bundle the code if necessary
                sh 'npm run build'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                // Run tests (e.g., using Mocha, Jest, etc.)
                sh 'npm test'
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Running code quality analysis...'
                // Run ESLint or another code quality tool
                sh 'npm run lint'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                // Run npm audit for vulnerability check
                sh 'npm audit'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                // Deployment to staging server, e.g., AWS
                sh 'aws deploy create-deployment --application-name myApp --deployment-group-name staging'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging environment...'
                // Run integration tests on staging
                sh 'npm run test:integration'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment...'
                // Deployment to production server, e.g., AWS
                sh 'aws deploy create-deployment --application-name myApp --deployment-group-name production'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up the workspace...'
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded!'
            mail to: 'developer@example.com',
                subject: "Pipeline Success: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: "The pipeline completed successfully at stage: ${currentBuild.currentResult}."
        }
        failure {
            echo 'Pipeline failed!'
            mail to: 'developer@example.com',
                subject: "Pipeline Failed: ${env.JOB_NAME} [${env.BUILD_NUMBER}]
