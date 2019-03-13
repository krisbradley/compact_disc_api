sh "export filename='SpringDataRestBoot-0.0.1-SNAPSHOT'"

pipeline {
    agent any
    tools {
        maven 'Maven3.5.4'
    }
    stages {
        stage('🛠 Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('📬 Delivery') {
            steps {
                sh 'aws s3 cp target/${filename}.jar s3://allstatejenkinss3/${filename}.jar --region eu-west-1'
            }
        }
        stage('✅ Deploy') {
            steps {
                sh '''
                    aws cloudformation create-stack --template-body  file://example.template --stack-name CdApi --capabilities CAPABILITY_IAM --region eu-west-1
                    aws cloudformation wait stack-create-complete --stack-name CdApi --region eu-west-1 
                    aws cloudformation describe-stack-events --stack-name CdApi --region eu-west-1
                '''
            }
        }
    }
}