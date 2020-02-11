pipeline {
    agent any
    stages {
        stage('Lint HTML') {
            steps {
                sh "tidy -q -e *.html"
            }
        }
        stage('Upload to AWS') {
            steps {
                withAWS(region: 'us-west-2', credentials: 'aws') {
                    s3Upload(file: 'index.html', bucket: 'udacity-aws-pipeline-bucket', path: 'index.html')
                }
            }
        }
    }

}