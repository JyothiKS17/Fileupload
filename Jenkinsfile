pipeline {
    agent { label 'jfrog' }

    options {
        skipDefaultCheckout(true)   // No SCM needed
    }

    parameters {
        file(
            name: 'UPLOAD_ZIP',
            description: 'Upload your zip file here'
        )
    }

    environment {
        WORKSPACE_ZIP = "${WORKSPACE}/uploaded.zip"
        DEST_DIR      = "${WORKSPACE}/unzipped"
    }

    stages {

        stage('Prepare Uploaded File on Agent') {
            steps {
                script {
                    if (!params.UPLOAD_ZIP?.trim()) {
                        error "No file uploaded. Please upload a ZIP file."
                    }

                    sh """
                        echo "Uploaded file reference  : ${params.UPLOAD_ZIP}"
                        echo "Copying uploaded file to agent workspace..."

                        cp "${params.UPLOAD_ZIP}" "${WORKSPACE_ZIP}"

                        echo "Verifying copied file:"
                        ls -lh "${WORKSPACE_ZIP}"
                    """
                }
            }
        }

        stage('Unzip File') {
            steps {
                sh """
                    mkdir -p "${DEST_DIR}"
                    unzip -o "${WORKSPACE_ZIP}" -d "${DEST_DIR}"

                    echo "Unzipped files:"
                    ls -l "${DEST_DIR}"
                """
            }
        }
    }

    post {
        success {
            echo "ZIP file uploaded and extracted successfully."
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}
