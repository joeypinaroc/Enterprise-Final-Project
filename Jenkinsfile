pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "us-east-2"
        AWS_DOCKER_REGISTRY = "819167064042.dkr.ecr.us-east-2.amazonaws.com"
        APP_NAME = "final-project"
    }

    stages {
        stage("Build") {
            agent {
                docker {
                    image 'node:20.11.0-bullseye'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    npm install
                    npm run build
                    ls -la
                '''
            }
        }

        stage("Test") {
            agent {
                docker {
                    image 'node:20.11.0-bullseye'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage("Build My Docker image") {
            agent {
                docker {
                    image 'amazon/aws-cli'
                    reuseNode true
                    args '-u root -v /var/run/docker.sock:/var/run/docker.sock --entrypoint=""'
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'for-final-project', passwordVariable: 'AWS_SECRET_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        amazon-linux-extras install docker
                        docker build -t $AWS_DOCKER_REGISTRY/$APP_NAME .
                        aws ecr get-login-password | docker login --username AWS --password-stdin $AWS_DOCKER_REGISTRY
                        docker push $AWS_DOCKER_REGISTRY/$APP_NAME:latest
                    '''
                }
            }
        }

        // stage("Deploy to AWS") {
        //     agent {
        //         docker {
        //             image 'amazon/aws-cli'
        //             reuseNode true
        //             args '-u root --entrypoint=""'
        //         }
        //     }
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: 'for-final-project', passwordVariable: 'AWS_SECRET_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
        //             sh '''
        //                 aws --version
        //                 yum install jq -y

        //                 LATEST_TD_REVISION=$(aws ecs )
        //             '''
        //         }
        //     }
        // }
    }
}