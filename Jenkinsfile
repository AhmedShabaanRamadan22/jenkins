pipeline {
    agent any

    stages {
        stage('Login and Deploy') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        echo "Logging in with user: $USERNAME"
                        curl -u $USERNAME:$PASSWORD https://example.com/login
                    '''
                }
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    args '-v /tmp:/tmp'
                }
            }
            steps {
                sh '''
                    echo "With docker"
                    echo "Ahmed shabaan" >ahmed.txt
                    node --version
                '''
            }
        }

        stage('Deploy Nginx Alpine Container') {
            steps {
                sh 'docker pull nginx:alpine'
                
                // Run the Nginx container
                sh '''
                    docker run -d \
                    --name my-nginx-alpine \
                    -p 5000:80 \
                    nginx:alpine
                '''
            }
        }
    }

    post {
        always {
            sh '''
                docker stop my-nginx-alpine
                docker rm my-nginx-alpine 
                cat ahmed.txt
            '''
        }
    }
}
