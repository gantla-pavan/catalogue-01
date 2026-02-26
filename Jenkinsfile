// pipeline {

//     agent {
//         label 'AGENT-1'
//     }

//     environment {
//         COURSE = "jenkins"
//         appVersion = ""
//     }

//     options {
//         timeout(time: 10, unit: 'MINUTES')
//         disableConcurrentBuilds()
//     }

    

//     stages {

//         stage('Read Version') {
//             steps {
//                 script {
//                     def packageJson = readJSON file: 'package.json'
//                     appVersion = packageJson.version // Assign value to the global variable
//                     echo "App version is: ${appVersion}"
//                 }
//             }
//         }

//         stage('Install Dependencies') {
//             steps () {
//                 script {
//                     sh """
//                         npm install
//                     """
//                 }
//             }
//         }

//         stage('Build') {
//             steps {
//                 sh """
//                 docker build -t catalogue:${appVersion} .
//                 docker images                   
//                 """
//             }
//         }

//         stage('Test') {
//             steps {
//                 sh """

//                 """
//             }
//         }

//         stage('Deploy') {
//             steps {
//                 sh """

//                 """
               
//             }
//         }
//     }

//     post {
//         always {
//             echo 'I will Always say Hello Again'
//             cleanWs()
//         }
//         success {
//             echo 'I will Run if Success'
//         }
//         failure {
//             echo 'I will Run if Failure'
//         }
//     }
// }




// 1. Define the variable here so it can be modified by stages


pipeline {
    agent {
        label 'AGENT-1'
    }

    environment {
        COURSE = "jenkins"
        appVersion = ""
        // Move appVersion out of here if you need to change its value dynamically
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
                    appVersion = packageJson.version 
                    echo "App version is: ${appVersion}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }

        stage('Build') {
            steps {
                // Use the variable we populated in the first stage
                sh """
                docker build -t catalogue:${appVersion} .
                docker images                   
                """
            }
        }
        
        // ... Test and Deploy
    }

    post {
        always {
            echo 'I will Always say Hello Again'
            cleanWs()
        }
    }
}