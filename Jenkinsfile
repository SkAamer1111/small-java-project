pipeline{
    agent { label 'node-1' }
    tools{
        jdk 'java-17'
        maven 'maven-3.9'
    }
    stages{
        stage ('CODE'){
            steps{
                git branch:'main' , url:'https://github.com/SkAamer1111/small-java-project.git'
            }
        }
        stage ('BUILD'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage ('TEST'){
            steps{
                sh 'mvn test'
            }
        }
        stage ('IMAGE BUILD'){
            steps{
                sh 'docker image build -t shaikhaamer/jenkins_docker_image:$BUILD_ID .'
            }
        }
        stage ('LOGIN'){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: 'docker_cred'
                    usernamevariable: 'DOCKER_USER'
                    passwordVariable: 'DOCKER_PASSWORD'
                )]){
                    sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }
        stage{
            steps{
                sh 'docker push shaikhaamer/jenkins_docker_image:$BUILD_ID'
            }
        }
        stage ('DEPLOY'){
            steps{
                sh 'docker container rm -f java-app'
                sh 'docker container run -d --name java-app -p 8081:8080 shaikhaamer/jenkins_docker_image:$BUILD_ID'
            }
        }
    }
    post {
        success{
            sh "echo 'pipeline sucessfuly executed'"
        }
        failure{
            sh "echo 'pipeline failed!!!!!!!!!1'"
        }
    }
    
}