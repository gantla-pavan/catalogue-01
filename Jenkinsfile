pipeline {

    agent {
        label 'AGENT-1'
    }

    environment {
        COURSE = "jenkins"
        APP_VERSION = ""
    }

    options {
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    stages {

        stage('Read Version') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    env.APP_VERSION = packageJSON.version
                    echo "App version: ${env.APP_VERSION}"
                }
            }
        }

        stage('Build') {
            steps {
                sh """
                    echo "Building version: $APP_VERSION"
                """
            }
        }

        stage('Test') {
            steps {
                sh """
                    echo "Testing"
                    echo $COURSE
                """
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    echo "Deploying"
                    echo $COURSE
                """
            }
        }

    }   // ✅ stages block closes here

    post {   // ✅ post is outside stages

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

}   // ✅ pipeline closes here