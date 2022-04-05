pipeline { 
    agent any 
 
    stages { 
        stage('Checkout Source') { /* primeira etapa */
            steps { 
                git url:'https://github.com/daniel10-souza/pedelogo-catalogo.git', branch: 'main' 
            }
        }

        stage('Build Image') { /* segunda etapa */
            steps {
                script {
                    dockerapp = docker.build("daniel10souza/api-produto:${env.BUILD_ID}", '-f src/PedeLogo.Catalogo.Api/Dockerfile .') /* comando de criação da imagem */
                } 
                
            }


        }

        stage('Push Image') { /* terceira etapa */
            steps {
                script { 
                        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
        stage('Deploy Kubernetes') { /* quarta etapa */
            agent { 
                kubernetes {
                    cloud 'kubernetes' 
                }
            }
            environment{
                tag_version = "${env.BUILD_ID}" 
            }
            steps {
                script {
                    sh 'sed -i "s/{{tag}}/$tag_version/g" ./k8s/api/deployment.yaml' 
                    sh 'cat ./k8s/api/deployment.yaml' 
                    kubernetesDeploy(configs: '**/k8s/**', kubeconfigId: 'kubeconfig') 
                }
            }
        }
    }
}
    
