pipeline {
    agent { label 'jfrog' }

    options {
        skipDefaultCheckout(true)  // no SCM needed
    }

    parameters {
        file(
            name: 'UPLOAD_ZIP',
            description: 'Upload any small ZIP file to test'
        )
    }

    stages {

        stage('Check if file is uploaded') {
            steps {
                script {
                    if (!params.UPLOAD_ZIP?.trim()) {
                        error "No file uploaded. Make sure you use 'Build with Parameters' and select a file."
                    } else {
                        echo "File parameter received: ${params.UPLOAD_ZIP}"

                        // Show where the file exists on the agent
                        sh """
                            echo "Agent workspace: ${WORKSPACE}"
                            echo "Listing file details from controller temp path:"
                            ls -lh "${params.UPLOAD_ZIP}"

                            # Copy file to workspace to make it permanent
                            cp "${params.UPLOAD_ZIP}" "${WORKSPACE}/uploaded_test.zip"

                            echo "File copied to workspace:"
                            ls -lh "${WORKSPACE}/uploaded_test.zip"
                        """
                    }
                }
            }
        }

        stage('Verify copied file') {
            steps {
                sh """
                    echo "Contents of workspace after copy:"
                    ls -l "${WORKSPACE}"
                """
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
