pipeline {
    environment {
        registry = "enfuras"
        prod = "84.201.173.215"
    }
    agent {
        docker {
            image 'enfuras/builder:latest'
            registryCredentialsId 'dockerhub'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage ('git') {
            steps {
                git(url: 'https://github.com/enfurec1do/user-registration.git', credentialsId: 'git')
            }
        }
        stage('build') {
            steps {
                sh 'ls'
                sh './mvnw -pl user-registration-application -am install'
                sh 'cp ./user-registration-application/target/user-registration-application-0.0.1-SNAPSHOT.war .'
            }
        }
        stage('make docker image') {
            steps {
                git(url: 'https://github.com/enfurec1do/lesson12.git', credentialsId: 'git')
                sh 'ls'
                sh 'cp ./app/Dockerfile .'
                script {
                   def image=docker.build("${env.registry}/user-registration:latest")
                   image.push()
                }
            }
        }
        
        stage('Run docker on prod') {
            steps {
                sshagent(['prod']){
                    sh 'docker run -d -p 8081:8080 $registry/user-registration:latest'
                }
            }
        }
        
    }
}