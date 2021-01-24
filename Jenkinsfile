
pipeline { 
    environment { 
        registry = "alishaukat718/demo" 
        registryCredential = 'dockerhub' 
        dockerImage = 'hello-world'

	PROJECT_ID = 'third-fire-260721'
        CLUSTER_NAME = 'k8s-cluster'
        LOCATION = 'europe-north1-a'
        CREDENTIALS_ID = 'kubernetes' 
    }
    agent any 
    stages { 
        stage('Scm Git') { 
            steps { 
                git 'https://github.com/alishaukat718/freestyledemo.git' 
            }
        } 
        stage('Build image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        stage('Push Docker Registry') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
            }
        } 
        stage('Depoy to K8s') {
            steps {
               echo "Deployment started ..."
	       sh 'ls -ltr'
	       sh 'pwd'
	       sh "sed -i 's/tagversion/$BUILD_NUMBER/g' deployment.yaml"
               step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
	       echo "Deployment Finished ..."
                    }
                }
        stage('Cleaning up') { 
            steps { 
                sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        } 
    }
}
