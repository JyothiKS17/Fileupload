pipeline {
    agent { label 'jfrog' }  // everything runs on JFrog agent

    parameters {
        file(name: 'UPLOAD_ZIP', description: 'Upload your zip file here')
    }

    stages {

        stage('Verify Upload') {
            steps {
                script {
                    def zipFile = "${WORKSPACE}/UPLOAD_ZIP"
                    echo "Workspace: $WORKSPACE"
                    echo "Uploaded file path: $zipFile"

                    // Check if file exists
                    if (!fileExists(zipFile)) {
                        echo "No file uploaded! Please use 'Build with Parameters'. Skipping unzip."
                        currentBuild.result = 'SUCCESS'
                        return
                    }

                    echo "File exists:"
                    sh "ls -l '$zipFile'"

                    // Check if file is a valid zip
                    def isZip = sh(
                        script: "file '$zipFile' | grep -i zip && echo 'YES' || echo 'NO'",
                        returnStdout: true
                    ).trim()

                    if (isZip == 'NO') {
                        echo "Uploaded file is not a valid zip. Skipping unzip."
                        currentBuild.result = 'SUCCESS'
                        return
                    }

                    // Show original filename
                    def zipName = sh(
                        script: "basename '$zipFile' .zip",
                        returnStdout: true
                    ).trim()
                    echo "Original file name (without extension): $zipName"

                    // Check if zip has content
                    def zipCount = sh(
                        script: 'unzip -l "$ZIP_FILE" | grep -v "^Archive" | grep -v "^--------" | grep -v "^$" | wc -l',
                        returnStdout: true
                    ).trim()

                    if (zipCount.toInteger() == 0) {
                        echo "Zip file is empty. Nothing to unzip."
                        currentBuild.result = 'SUCCESS'
                        return
                    }

                    echo "Zip file contains $zipCount file(s)."

                    // Set environment for unzip stage
                    env.DEST_DIR = "${WORKSPACE}/unzipped/${zipName}"
                }
            }
        }

        stage('Unzip File') {
            steps {
                script {
                    sh """
                        mkdir -p "$DEST_DIR"
                        unzip -o "$WORKSPACE/UPLOAD_ZIP" -d "$DEST_DIR"
                        ls -l "$DEST_DIR"
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
