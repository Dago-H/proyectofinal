pipeline{
    agent any

    environment{
        IMAGE_NAME = 'proyectofinal'
        IMAGE_NAME_TEST = 'proyectofinaltesting'
        DOCKER_USERNAME = 'cursodvops'
        DOCKER_CREDENTIALS = ''
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

        stage('Deploy'){
            steps{
                script{
                    docker.build("IMAGE_NAME_TEST","--file=Dockerfile .")
                }
            }
        }
    }
}