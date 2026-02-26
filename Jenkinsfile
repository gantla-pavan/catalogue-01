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
                    // sh 'ls -l'

                    def packageJSON = readJSON file: './package.json'

                    // if (packageJSON?.version == null) {
                    //     error "Version key not found in package.json"
                    // }

                    env.APP_VERSION = packageJSON.version

                    echo "App version is: ${env.APP_VERSION}"
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