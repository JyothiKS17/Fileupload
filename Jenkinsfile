pipeline {
    agent { label 'jfrog' }

    options {
        skipDefaultCheckout(true)  // No SCM needed
    }

    parameters {
        file(
            name: 'UPLOAD_ZIP',
            description: 'Upload your ZIP file to test'
        )
    }

    stages {
        stage('Test File Upload') {
            steps {
                script {
                    if (!params.UPLOAD_ZIP?.trim()) {
                        error "No file uploaded. Make sure you use 'Build with Parameters'."
                    } else {
                        echo "File upload received: ${params.UPLOAD_ZIP}"

                        // Copy file to agent workspace so we can see it
                        sh """
                            cp "${params.UPLOAD_ZIP}" "${WORKSPACE}/uploaded.zip"
                            echo "File copied to workspace:"
                            ls -lh "${WORKSPACE}/uploaded.zip"
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ File upload test completed successfully."
        }
        failure {
            echo "❌ File upload test failed."
        }
    }
}
