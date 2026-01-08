pipeline {
    agent { label 'jfrog' }  // Run on JFrog server agent

    parameters {
        file(name: 'UPLOAD_ZIP', description: 'Upload your zip file here')
    }

    environment {
        DEST_DIR = "${WORKSPACE}/unzipped"   // Destination folder for unzipped files
        ZIP_FILE = "${WORKSPACE}/UPLOAD_ZIP" // Path to uploaded file in workspace
    }

    stages {

        stage('Verify Upload') {
            steps {
                sh '''
                    echo "Workspace: $WORKSPACE"
                    echo "Uploaded file (workspace path): $ZIP_FILE"

                    # Check if the file exists
                    if [ ! -f "$ZIP_FILE" ]; then
                        echo "Error: No file uploaded! Please use 'Build with Parameters'."
                        exit 1
                    fi

                    echo "File exists!"
                    ls -l "$ZIP_FILE"

                    # Show original filename
                    echo "Original file name: $(basename "$ZIP_FILE")"
                '''
            }
        }

        stage('Unzip File') {
            steps {
                sh '''
                    mkdir -p "$DEST_DIR"                 # Create destination folder
					unzip -o "$ZIP_FILE" -d "$DEST_DIR"  # Extract uploaded zip
                    ls -l "$DEST_DIR"                     # Show extracted files
                '''
                echo "File unzipped to: $DEST_DIR"
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
