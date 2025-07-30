pipeline{
    agent any

    environment {
        KUBECONFIG = 'C:\\Program Files\\Jenkins\\kubeconfig.yaml'
        IMAGE_NAME = 'proyectofinal'
        IMAGE_NAME_TEST = 'proyectofinaltesting'
        DOCKER_USERNAME = 'cursodvops'
        DOCKER_CREDENTIALS = 'docker-hub-credentials'
        DOCKER_IMAGE = "${DOCKER_USERNAME}/${IMAGE_NAME}"
    }

    triggers{
        githubPush()
    }

    stages{
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Dago-H/proyectofinal.git'
            }
        }

        stage('Test'){
            steps{
                script{
                    docker.build("proyectofinaltesting","--file=Dockerfile.test .")
                }
            }
        }

        stage('Push'){
            steps{
                script{
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
                        image.push()
                    }
                }
            }
        }

        stage('Deploy'){
            steps{
                script{
                    bat 'kubectl apply -f k8s/namespace.yaml'
                    bat 'kubectl apply -f k8s/deployment.yaml'
                    bat 'kubectl apply -f k8s/service.yaml'
                }
            }
        }

     }

        

    
}