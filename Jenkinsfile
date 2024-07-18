pipeline {
    tools {
        maven "m3"
        'org.jenkinsci.plugins.docker.commons.tools.DockerTool' "docker"
    }
    stages {
        stage('git') {
            agent build
            steps {
                git 'https://github.com/boxfuse/boxfuse-sample-java-war-hello.git'
            }
        }
        stage('build') {
            agent build
            steps {
                sh 'mvn package'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.war'
                }
            }
        }
        stage('dockerfile') {
            agent build
            steps {
                git branch: 'main', url: 'https://github.com/1315a/hw11_pipeline.git'
            }
        }
         stage('Build Docker image') {
            agent build
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        def dockerImage = docker.build("1315a/hw11:latest", ".")
                        dockerImage.push()
                    }
                }
            }
        }
         stage('Start Docker image') {
            agent prod
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        def dockerImage = docker.pull("1315a/hw11:latest", ".")
                        dockerImage.start()
                    }
                }
            }
        }
    }
}
