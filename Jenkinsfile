//Jenkins agents cannot accept direct uploads; all uploads go through the Jenkins controller. So we are uploading the file from local system to jenkins server

pipeline {  //declarative pipeline
    agent { label 'jfrog' }  //job runs in jfrog node/slave
    parameters {
        file(name: 'UPLOAD_ZIP', description: 'Upload your zip file here') //When you click Build with Parameters, Jenkins shows a file upload option and upload

        //Jenkins stores the uploaded file in the workspace of the node. The parameter MY_ZIP contains the full path to the uploaded file /var/lib/jenkins/workspace/
    }
    environment {
        DEST_DIR = "${WORKSPACE}/unzipped"  //Defines a reusable variable. This is the directory where the zip contents will be extracted
    }
    stages {
        stage('Show uploaded file') {  //Verify that the file upload worked correctly
            steps {
                script {        //We are using Groovy logic (new File()). Declarative pipeline needs script {} for advanced logic
                    def filePath = params.UPLOAD_ZIP   //Full path of the uploaded zip file in the workspace
                     def fileName = sh(
                        script: "basename ${filePath}",
                        returnStdout: true
                    ).trim()   //Jenkins doesn’t give the filename directly. This extracts.
                    echo "Uploaded file path: ${filePath}"
                    echo "Original file name: ${fileName}"   // Debugging, Confirming the correct file is uploaded
                    sh "ls -l ${filePath}" //File exists, File size and permissions are visible, Upload is successful
                }
            }
        }
    }
}
