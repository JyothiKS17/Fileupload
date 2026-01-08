pipeline {
    agent { label 'jfrog' } // No default agent; we will assign per stage

    parameters {
        file(name: 'UPLOAD_ZIP', description: 'Upload your zip file here')
    }

    environment {
        DEST_DIR = "unzipped"   // Folder on agent to unzip files
    }

    stages {

        stage('Prepare Uploaded File on Controller') {
            agent { label 'master' } // run on Jenkins controller
            steps {
                script {
                    echo "Controller workspace: $WORKSPACE"

                    // The uploaded file is automatically in the workspace with the parameter name
                    def uploadedFile = "${WORKSPACE}/UPLOAD_ZIP"

                    if (!fileExists(uploadedFile)) {
                        error "No file uploaded! Please use 'Build with Parameters'."
                    }

                    echo "Uploaded file exists on controller:"
                    sh "ls -l ${uploadedFile}"

                    // Save it to be sent to agent
                    stash includes: 'UPLOAD_ZIP', name: 'uploaded-zip'
                    echo "File stashed for agent"
                }
            }
        }

        stage('Process on JFrog Agent') {
            agent { label 'jfrog' } // run on JFrog agent
            steps {
                script {
                    echo "JFrog agent workspace: $WORKSPACE"

                    // Retrieve the file from controller
                    unstash 'uploaded-zip'

                    echo "File received on agent:"
                    sh "ls -l UPLOAD_ZIP"

                    // Show original filename
                    sh 'echo "Original file name: $(basename UPLOAD_ZIP)"'

                    // Unzip
                    sh """
                        mkdir -p $DEST_DIR
                        unzip -o UPLOAD_ZIP -d $DEST_DIR
                        ls -l $DEST_DIR
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
