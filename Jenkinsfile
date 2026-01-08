pipeline {
    agent any  // Use any available agent; can be your Jenkins master or node

    parameters {
        file(name: 'MY_FILE', description: 'Upload a file for processing')
        string(name: 'ENV', defaultValue: 'dev', description: 'Environment')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the same repo
                checkout scm
            }
        }

        stage('Show Uploaded File') {
            steps {
                echo "Uploaded file: ${params.MY_FILE}"
                sh "ls -l ${params.MY_FILE}"
            }
        }

        stage('Build') {
            steps {
                echo "Building for environment: ${params.ENV}"
            }
        }
    }
}
