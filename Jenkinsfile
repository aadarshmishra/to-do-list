pipeline {
    agent any
    
    stages {
        stage('Git Pull') {
            steps {
                git branch: 'main', credentialsId: 'git-creds', url: 'https://github.com/aadarshmishra/to-do-list.git'
            }
        }
        stage('Maven Build') {
            steps {
                echo "Building"
            }
        }
        stage('Docker Build') {
            steps {
                sh "docker build -t todolistimg ."
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh "docker tag todolistimg aadarsh96/todolistimg:latest"
                    sh 'docker push aadarsh96/todolistimg:latest'
                }
            }
        }
        stage('Ansible Pull and Run Docker Image') {
            steps {
                ansiblePlaybook becomeUser: null, colorized: true, disableHostKeyChecking: true, inventory: 'inventory', playbook: 'deploy-image.yml', sudoUser: null
            }
        }
    }
}
