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

    // parameters {
    //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    //     text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
    //     booleanParam(name: 'DEPLOY', defaultValue: false, description: 'Toggle this value')
    //     choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
    //     password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    // }

    stages {

        stage('Read Version') {
            steps {
                script {
                    // Read package.json file
                    def packageJSON = readJSON file: 'package.json'
                    
                    // Store version in environment variable
                    env.APP_VERSION = packageJSON.version
                    echo "App version: ${env.APP_VERSION}"
                }
            }
        }

        stage('Build') {
            steps {
                sh """
                    echo "Building version: $APP_VERSION"
                    echo "Hello ${params.PERSON}"
                    echo "Biography: ${params.BIOGRAPHY}"
                    echo "Choice: ${params.CHOICE}"
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
                    echo "Testing"
                    echo $COURSE
                """
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
