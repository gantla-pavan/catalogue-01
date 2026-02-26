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
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version // Assign value to the global variable
                    echo "App version is: ${appVersion}"
                }
            }
        }

        stage('Install Dependencies') {
            steps () {
                script {
                    sh """
                        npm install
                    """
                }
            }
        }

        stage('Build') {
            steps {
                sh """
                docker build -t catalogue:${appVersion} .
                docker images                   
                """
            }
        }

        stage('Test') {
            steps {
                sh """

                """
            }
        }

        stage('Deploy') {
            steps {
                sh """

                """
               
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