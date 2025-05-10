pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'il-central-1'
        FUNCTION_NAME = 'imtech-dor-from-gh'
        LAMBDA_CODE_DIR = 'lambda-code'
    }

    stages {
        stage('Zip Lambda Code') {
            steps {
                 sh '''
                    rm -f lambda.zip
                    cd lambda-code
                    zip -r ../lambda.zip .
                '''
            }
        }

        stage('Upload to AWS Lambda') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'imtech'
                ]]) {
                    sh '''
                        aws lambda update-function-code \
                          --function-name $FUNCTION_NAME \
                          --zip-file fileb://lambda.zip
                    '''
                }
            }
        }
    }
}

