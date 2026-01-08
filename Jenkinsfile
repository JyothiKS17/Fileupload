pipeline {
    agent { label 'jfrog' }  // everything runs on JFrog agent

    parameters {
        file(name: 'UPLOAD_ZIP', description: 'Upload your zip file here')
    }

    environment {
        DEST_DIR = "${WORKSPACE}/unzipped" // Folder to unzip files
        ZIP_FILE = "${WORKSPACE}/UPLOAD_ZIP" // Path to uploaded file
    }

    stages {

        stage('Verify Upload') {
            steps {
                script {
                    echo "Workspace: $WORKSPACE"
                    echo "Expected uploaded file path: $ZIP_FILE"

                    // Use shell to check if file exists
                    def fileExists = sh(
                        script: "[ -f '$ZIP_FILE' ] && echo 'YES' || echo 'NO'",
                        returnStdout: true
                    ).trim()

                    if (fileExists == 'NO') {
                        echo "No file uploaded! Please use 'Build with Parameters'. Skipping unzip."
                        currentBuild.result = 'SUCCESS'
                        return
                    }

                    echo "File exists!"
                    sh "ls -l '$ZIP_FILE'"

                    // Show original filename
                    sh 'echo "Original file name: $(basename \"$ZIP_FILE\")"'
                }
            }
        }

        stage('Unzip File') {
            when {
                expression { fileExists("$ZIP_FILE") == 'YES' } // Only unzip if file exists
                 }
            steps {
                sh """
                    mkdir -p "$DEST_DIR"
                    unzip -o "$ZIP_FILE" -d "$DEST_DIR"
                    ls -l "$DEST_DIR"
                """
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
