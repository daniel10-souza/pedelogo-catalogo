pipeline { /* os estágios da pipeline estarão nesse bloco */
    agent any /* agente é 'quem' vai executar a pipeline */

    stages { /* inicia a criação das etapas */
        stage('Checkout Source') { /* primeira etapa */
            steps { /* define os passos a serem executados nessa etapa */
                git url:'https://github.com/breinerHenrique/pedelogo-catalogo.git', branch: 'main' /* 'pega' a fonte do projeto */
            }
        }

        stage('Build Image') { /* segunda etapa */
            steps {
                script {
                    dockerapp = sudo docker.build("breinerhenrique/api-produto:${env.BUILD_ID}", '-f src/PedeLogo.Catalogo.Api/Dockerfile .') /* comando de criação da imagem */
                } 
                
            }


        }

        stage('Push Image') {
            steps {
                script { /* registra no docker hub com a credencial criada no Jenkins, em seguida, faz o push das duas imagens */
                        sudo docker.withRegistry('https://registry.hub.docker.com', 'docker_hub') {
                        sudo dockerapp.push('latest')
                        sudo dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    }
}
    
