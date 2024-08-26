pipeline {
    agent any

    stages {
        stage('Login and Deploy') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        echo "Logging in with user: $USERNAME"
                        echo $PASSWORD | docker login -u "$USERNAME" --password-stdin
                    '''
                }
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:alpine'
                }
            }
            steps {
                sh '''
                    echo "With docker"
                    echo "Ahmed shabaan" >ahmed.txt
                    node --version
                    stash name: 'myname-file', includes: 'ahmed.txt'

                '''
            }
        }

        stage('Deploy Nginx Alpine Container') {
            steps {
                sh 'docker pull nginx:alpine'
                                sh '''
                    docker run -d \
                    --name my-nginx-alpine \
                    -p 6000:80 \
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
                unstash 'myname-file'
                cat ahmed
            '''
        }
    }
}
