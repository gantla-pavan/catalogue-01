pipeline {

    //  This is pre-build section
    agent {
        label 'AGENT-1'
    }

    environment {
        COURSE = "jenkins"
        appVersion = ""
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

    // This is build section
    stages {
        stage ( 'Read Version' ) {
            steps {
                script {
                    
                    def Package.JSON = readJSON file: 'package.json'
                    appVersion      = packageJSON.version
                    echo  "app version: ${appversion}"


                    
                }
            }

        }

        stage('Build') {
            steps {
                sh """
                    
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

            when {
                expression { "$params.DEPLOY" == "true" }
            }

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
