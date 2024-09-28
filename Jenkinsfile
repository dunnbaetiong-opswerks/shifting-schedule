pipeline {
    agent {
        label 'docker-host'
    }
 
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/hanstabotabo/shifting-schedule.git'
                /*sh '''
                if (fileExists('shifting-schedule')) {
                    dir('shifting-schedule') {
                        git fetch --all'
                        sh 'git reset --hard origin/main'
                    }
                } else {
                    git clone https://github.com/hanstabotabo/shifting-schedule.git
                }
                ''
                */
            }
        }
        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                sh '''
                echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                sudo docker pull docker.io/hanstabotabo/mini-proj
                '''
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'kubectl apply -f deployment.yaml --validate=false'
                }
            }
        }
    }
}
