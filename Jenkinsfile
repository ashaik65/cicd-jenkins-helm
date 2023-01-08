pipeline {
    agent any
    stages {
        stage ('Build maven') {
            steps {
                sh 'pwd'
                sh 'mvn clean install package'
            }
        }
        stage ('Copy Artifact') {
            steps {
                sh 'pwd'
                sh 'cp -r target/*.jar docker'
            }
        }
        stage ('Build docker image') {
            steps {
                script {
                    def customImage = docker.build('srijananis123/petclinic', "./docker")
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                    customImage.push('latest')    
                    }
                }
            }
        }
        stage ('Build on kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh 'pwd'
                    sh 'cp -R helm/* .'
                    sh 'ls -ltrh'
                    sh 'pwd'
                    sh '/usr/local/bin/helm upgrade --install petclinic-app petclinic --set image.repository=srijananis123/petclinic --set image.tag=latest'
                }
            }
        }
    }
}



