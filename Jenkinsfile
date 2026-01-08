pipeline {
    agent { label 'jfrog' } // Run on JFrog server agent

    parameters {
        file(name: 'UPLOAD_ZIP', description: 'Upload your zip file here')
    }

    environment {
        DEST_DIR = "${WORKSPACE}/unzipped"      // Destination folder for unzipped files
        ZIP_FILE = "${WORKSPACE}/UPLOAD_ZIP"    // Path to uploaded file in workspace
    }

    stages {

        stage('Verify Upload') {
            steps {
                script {
                    echo "Workspace: $WORKSPACE"
                    echo "Uploaded file (workspace path): $ZIP_FILE"

                    // Check if file exists
                    if (!fileExists(env.ZIP_FILE)) {
                        error "No file uploaded! Please use 'Build with Parameters'."
                    }

                    // Capture original filename safely
                    def originalName = sh(
                        script: "basename $ZIP_FILE",
                        returnStdout: true
                    ).trim()

                    echo "Original file name: ${originalName}"

                    // List file details
                    sh "ls -l $ZIP_FILE"
                }
            }
        }

        stage('Unzip File') {
            steps {
                script {
                    sh """
                        mkdir -p $DEST_DIR               # Create destination folder
                        unzip -o $ZIP_FILE -d $DEST_DIR  # Extract uploaded zip
                        ls -l
						"""
                    echo "File unzipped to: $DEST_DIR"
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully ✅"
        }
        failure {
            echo "Pipeline failed ❌"
        }
    }
}
