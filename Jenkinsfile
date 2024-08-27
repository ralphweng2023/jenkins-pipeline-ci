pipeline {
    agent any

    stage('Clean Workspace') {
        steps {
            cleanWs()  // Clean the workspace before starting the build
        }
    }
    stages {
        stage('Test') {
            steps {
                echo 'Pipeline is running correctly!'
            }
        }
    }
}
