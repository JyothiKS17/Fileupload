pipeline {
    agent { label 'jfrog' }

    options {
        skipDefaultCheckout(true)   // No SCM checkout needed
    }

    parameters {
        file(
            name: 'UPLOAD_ZIP',
            description: 'Upload your WinRAR ZIP file here'
        )
    }

    environment {
        WORKSPACE_ZIP = "${WORKSPACE}/uploaded.zip"
        DEST_DIR      = "${WORKSPACE}/unzipped"
    }

    stages {

        stage('Validate and Copy Uploaded File') {
            steps {
                script {
                    if (!params.UPLOAD_ZIP?.trim()) {
                        // Optional: fail or just warn
                        error "No file uploaded. Please upload a ZIP file."
                    } else {
                        echo "File uploaded: ${params.UPLOAD_ZIP}"

                        // Copy the uploaded file to agent workspace
                        sh """
                            cp "${params.UPLOAD_ZIP}" "${WORKSPACE_ZIP}"
                            echo "Copied uploaded file to agent workspace:"
                            ls -lh "${WORKSPACE_ZIP}"
                        """
                    }
                }
            }
        }

        stage('Unzip File') {
            steps {
                script {
                    sh """
                        mkdir -p "${DEST_DIR}"
                        echo "Unzipping ${WORKSPACE_ZIP} to ${DEST_DIR}..."

                        # Use 7z for WinRAR-compatible ZIP files
                        7z x "${WORKSPACE_ZIP}" -o"${DEST_DIR}"

                        echo "Contents of unzipped directory:"
                        ls -l "${DEST_DIR}"
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ ZIP file uploaded and extracted successfully."
        }
        failure {
            echo "❌ Pipeline failed. Check logs for details."
        }
    }
}
