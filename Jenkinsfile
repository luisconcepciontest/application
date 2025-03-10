pipeline {
    agent any
    environment {
        //be sure to replace "luisconcepciontest" with your own Docker Hub username | test webhooks2
        DOCKER_IMAGE_NAME = "luisconcepciontest/timeoff-management-gorilla"
    }
    stages {

        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME) //Create and tag image with the name luisconcepciontest/timeoff-management-gorilla using the Dockerfile in the repository
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps {
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'timeoff-management-gorilla.yml',
                    enableConfigSubstitution: true
                )
            }
        }

    }
}
