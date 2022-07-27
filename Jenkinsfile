pipeline{
    agent{
        label ('JAVA_11')
    }
    environment {
        //AWS_ACCOUNT_ID='aws account number'
        //AWS_DEFAULT_REGION='region'
        //IMAGE_REPO-NAME='jenkins/spring-pet'
        //IMAGE_TAG='1.0'
        //REPOSITORY_URI='${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO-NAME}'
        registry='pvenkateswararo/spring-pet'
        dockerImage=''
        registryCredential='Dockerhub_id'
    }
    stages{
        stage('scm'){
            steps{
                git 'https://github.com/venkatesh-pogula/dockertojenkins.git'
            }
        }
        stage('build image'){
            steps{
                sh 'sudo docker build -t ${registry}:2.10 .'
            }
        }
        stage('push image into Dockerhub'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'Dockerhub_id', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]){
                    sh "sduo docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh 'sudo docker push pvenkateswarrao/spring-pet:2.10'
                }
            }
        }
        stage('deploy in k8s'){
            steps{
                sh 'kubectl apply -f spring-pet-deploy.yml'
            }
        }
        stage('deploy to production'){
            steps{
                sh 'kubectl create deployment prod-pod --image=pvenkateswarrao/spring-pet:2.10  -n=production --replicas=2'
            }
        }
        stage('deploy to develper'){
            steps{
                sh 'kubectl create deployment dev-pod --image=pvenkateswarrao/spring-pet:2.10  -n=developer --replicas=2'
            }
        }
        stage('deploy to test'){
            steps{
                sh 'kubectl create deployment test-pod --image=pvenkateswarrao/spring-pet:2.10  -n=test --replicas=2'
            }
        }

    }
}

