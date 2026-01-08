pipeline {
    agent none

    stages {

        stage('Upload on Controller') {
            agent { label 'master' }
            steps {
                sh '''
                    echo "Controller workspace:"
                    ls -l $WORKSPACE

                    if [ ! -f "$WORKSPACE/UPLOAD_ZIP" ]; then
                        echo "UPLOAD FAILED ON CONTROLLER"
                        exit 1
                    fi
                '''
                stash name: 'uploadedZip', includes: 'UPLOAD_ZIP'
            }
        }

        stage('Use on JFrog Agent') {
            agent { label 'jfrog' }
            steps {
                unstash 'uploadedZip'
                sh '''
                    echo "JFrog agent workspace:"
                    ls -l $WORKSPACE
                '''
            }
        }
    }
}
