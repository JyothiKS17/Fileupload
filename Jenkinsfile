pipeline {
    agent none

    stages {
        stage('Verify Upload on Controller') {
            agent { label 'master' }
            steps {
                sh '''
                    echo "Running on controller"
                    echo "WORKSPACE is: $WORKSPACE"
                    echo "Listing workspace contents:"
                    ls -l

                    echo "Checking uploaded file..."
                    if [ -f "$WORKSPACE/UPLOAD_ZIP" ]; then
                        echo "UPLOAD SUCCESS ON CONTROLLER"
                        ls -lh "$WORKSPACE/UPLOAD_ZIP"
                    else
                        echo "UPLOAD FAILED"
                        echo "Expected file at: $WORKSPACE/UPLOAD_ZIP"
                        exit 1
                    fi
                '''
            }
        }
    }
}
