pipeline {
   agent any
   environment{
    AWS_REGION = 'us-east-1'
    IMAGE_ECR_REPO = '585008070473.dkr.ecr.us-east-1.amazonaws.com/jenkins-ci'
    ECR_REPO = '585008070473.dkr.ecr.us-east-1.amazonaws.com'
   }
   stages{
    stage('CodeScanNormal'){
        steps{
            sh 'trivy fs . -o result.html'
            
                    }

    }
    stage('dockerLogin') {
        steps{
            sh "aws ecr get-login-password --region $AWS_REGION | \
            docker login --username AWS \
             --password-stdin 585008070473.dkr.ecr.us-east-1.amazonaws.com"
        }
    }
    stage('dockerImageBuild'){
        steps{
            sh 'docker build -t jenkins-ci .'
            sh 'docker build -t imageversion .'
        }
}
    stage('dockerImgeTag') {
        steps{
            sh "docker tag jenkins-ci:latest\
             $IMAGE_ECR_REPO:latest"
            sh "docker tag imageversion $IMAGE_ECR_REPO:v1.$BUILD_NUMBER"
        }
    } 
    stage('pushImage'){
        steps{
            sh "docker push \
            $ECR_REPO/jenkins-ci:latest"
            sh "docker push \
            $ECR_REPO/jenkins-ci:v1.$BUILD_NUMBER"
        }
    }
   } 

}