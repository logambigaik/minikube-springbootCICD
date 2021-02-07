pipeline {
    agent any
    environment {
        registry = "naresh240/springboot-k8s:latest"
        registryCredential = 'docker-credentials'
        dockerImage = ''
    }
    stages {
        stage ('checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                		branches: [[name: '*/main']],
                		doGenerateSubmoduleConfigurations: false, extensions: [],
                		submoduleCfg: [], 
                		userRemoteConfigs: [[url: 'https://github.com/Naresh240/springbootCICD-demo.git']]])
			}
		}
        stage('Build Maven'){
            steps{
                sh 'mvn clean install -DskipTests=true'
            }
        }
        stage ('Build docker image') {
            steps {
                script {
                    dockerImage=docker.build registry
                }
            }
        }
        stage('Upload Image') {
            steps{   
                script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                    }
                }
            }
        }
        stage ('K8S Deploy') {
            steps {
                script {
    				withCredentials([kubeconfigFile(credentialsId: 'kubeConfig', variable: 'KUBECONFIG')]) {
                        sh '''
                            kubectl apply -f deployement.yml
                            kubectl apply -f service.yml
                        '''
    				}
                }
            }
        }
	}
}
