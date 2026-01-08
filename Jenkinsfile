pipeline {
    agent none

    stages {
        stage('Verify Upload on Controller') {
            agent { label 'master' }   // controller
            steps {
                sh '''
                    echo "Running on controller"
                    echo "Workspace: $WORKSPACE"
                    ls -l

                    if [ -f "$WORKSPACE/UPLOAD_ZIP" ]; then
                        echo "UPLOAD SUCCESS ON CONTROLLER"
                        ls -lh "$WORKSPACE/UPLOAD_ZIP"
                    else
                        echo "UPLOAD FAILED"
                        exit 1
                    fi
                '''
            }
        }
    }
}
