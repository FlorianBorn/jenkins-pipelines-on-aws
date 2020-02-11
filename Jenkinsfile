pipeline {
    agent any
    stages {
        stage('Upload to AWS') {
            steps {
                withAWS(region: 'us-west-2', credentials: 'aws') {
                    s3Upload(file: 'index.html', bucket: 'udacity-aws-pipeline-bucket', path: 'index.html')
                }
            }
        }
    }

}