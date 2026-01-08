pipeline {
    agent { label 'jfrog' }

    parameters {
        file(name: 'UPLOAD_ZIP', description: 'Upload your zip file here')
    }

    environment {
        DEST_DIR = "${WORKSPACE}/unzipped"
        ZIP_FILE = "${WORKSPACE}/UPLOAD_ZIP"
    }

    stages {
        stage('Show uploaded file') {
            steps {
                sh """
                    echo "Workspace: $WORKSPACE"
                    echo "Uploaded file (Jenkins workspace path): $ZIP_FILE"
                    ls -l $ZIP_FILE
                    echo "Original file name: $(basename $ZIP_FILE)"
                """
            }
        }

        stage('Unzip file') {
            steps {
                sh """
                    mkdir -p $DEST_DIR
                    unzip -o $ZIP_FILE -d $DEST_DIR
                    ls -l $DEST_DIR
                """
                echo "File unzipped to: $DEST_DIR"
            }
        }
    }
}
