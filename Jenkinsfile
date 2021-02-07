pipeline{
    agent any
    parameters {
	    string(name: 'ImageTag', description: "Name of the docker build")
    }
    tools { 
        maven 'maven3' 
    }
    stages{
        stage('git checkout'){
            steps{
                git 'https://github.com/Naresh240/spring-boot-hello.git'
            }
        }
        stage('Build Maven'){
            steps{
                sh 'mvn clean install -DskipTests=true'
            }
        }
        stage("Docker Build") {
	        steps {
	            sh 'docker build -t naresh240/spring-boot-hello:${ImageTag} .'
	        }
        }
        stage("Docker Push") {
	        steps {
               withCredentials([usernamePassword(credentialsId: 'docker_credentials', passwordVariable: 'docker_password', usernameVariable: 'docker_username')]) {
                   sh 'docker login -u ${docker_username} -p ${docker_password}'
               }
               sh 'docker push naresh240/spring-boot-hello:${ImageTag}'
           }
        }
    }
}
