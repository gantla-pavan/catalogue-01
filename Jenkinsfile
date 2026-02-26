pipeline {

    agent {
        label 'AGENT-1'
    }

    options {
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    stages {

        stage('Read Version') {
            steps {
                script {

                    // Debug: confirm file exists
                    sh 'ls -l'

                    def packageJSON = readJSON file: './package.json'

                    if (packageJSON?.version == null) {
                        error "Version key not found in package.json"
                    }

                    env.APP_VERSION = packageJSON.version

                    echo "App version is: ${env.APP_VERSION}"
                }
            }
        }

        stage('Build') {
            steps {
                echo "Building version: ${env.APP_VERSION}"

                sh """
                    echo "Shell sees version: $APP_VERSION"
                """
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Testing application"'
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo "Deploying application"'
            }
        }
    }

    post {
        always {
            echo 'I will Always say Hello Again'
            cleanWs()
        }
        success {
            echo 'I will Run if Success'
        }
        failure {
            echo 'I will Run if Failure'
        }
    }
}