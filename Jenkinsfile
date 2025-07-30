pipeline{
    agent any

    environment {
        KUBECONFIG = 'C:\\Program Files\\Jenkins\\kubeconfig.yaml'
        IMAGE_NAME = 'proyectofinal'
        IMAGE_NAME_TEST = 'proyectofinaltesting'
        DOCKER_USERNAME = 'cursodvops'
        DOCKER_CREDENTIALS = 'docker-hub-credentials'
        DOCKER_IMAGE = "${DOCKER_USERNAME}/${IMAGE_NAME}"
        EMAIL_FROM = 'trabajo.2024.comun@gmail.com'
        EMAIL_TO = 'trabajo.2024.comun@gmail.com'
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

    post {
        failure {
            mail to: EMAIL_TO,
                from: EMAIL_FROM,
                subject: "Build Failed in Jenkins: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Something is wrong with the build. Please check the console output at ${env.BUILD_URL}" 
        }

    
}