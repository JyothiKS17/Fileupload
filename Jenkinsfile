stage('Show uploaded file') {
    steps {
        script {
            echo "Workspace: $WORKSPACE"
            echo "Uploaded file (workspace path): $ZIP_FILE"

            // Get original filename safely
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
