pipeline {
    agent any

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
    }
}