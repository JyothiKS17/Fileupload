pipeline {
    agent any

    stages {
        stage('Verify Upload') {
            steps {
                sh '''
                    echo "Workspace: $WORKSPACE"
                    echo "Files in workspace:"
                    ls -l

                    if [ -f "$WORKSPACE/UPLOAD_ZIP" ]; then
                        echo "Upload SUCCESS"
                        ls -lh "$WORKSPACE/UPLOAD_ZIP"
                    else
                        echo "Upload FAILED"
                        exit 1
                    fi
                '''
            }
        }
    }
}
