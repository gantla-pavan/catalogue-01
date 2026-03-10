// pipeline {
//     agent {
//         label 'AGENT-1'
//     }

//     environment {
//         ACC_ID    = "690509490317"
//         PROJECT   = "roboshop"
//         COMPONENT = "catalogue"
//         SONAR_AUTH_TOKEN = credentials('sonar-token')
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
//                     env.APP_VERSION = packageJson.version
//                     echo "App version is: ${env.APP_VERSION}"
//                 }
//             }
//         }

//         stage('Install Dependencies') {
//             steps {
//                 sh 'npm install'
//             }
//         }

//         stage('Unit Test') {
//             steps {
//                 sh 'npm test'
//             }
//         }

//         // stage('Sonar Scan') {
//         //     steps {
//         //         withSonarQubeEnv('SonarCloud') {
//         //             withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
//         //                 sh '''
//         //                 npx sonar-scanner \
//         //                 -Dsonar.projectKey=gantla-pavan_catalogue-01 \
//         //                 -Dsonar.organization=gantla-pavan \
//         //                 -Dsonar.sources=. \
//         //                 -Dsonar.host.url=https://sonarcloud.io \
//         //                 -Dsonar.token=$SONAR_TOKEN
//         //                 '''
//         //             }
//         //         }
//         //     }
//         // }

//         // stage('Quality Gate') {
//         //     steps {
//         //         timeout(time: 2, unit: 'MINUTES') {
//         //             script {
//         //                 def qg = waitForQualityGate()

//         //                 if (qg.status != 'OK') {
//         //                     error "Pipeline aborted due to Quality Gate failure: ${qg.status}"
//         //                 } else {
//         //                     echo "Quality Gate passed: ${qg.status}"
//         //                 }
//         //             }
//         //         }
//         //     }
//         // }

//        stage('Dependabot Security Gate') {
//        steps {
//         withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')]) {
//             script {

//                 sh '''
//                 echo "Fetching Dependabot alerts..."

//                 curl -s \
//                 -H "Authorization: Bearer $GITHUB_TOKEN" \
//                 -H "Accept: application/vnd.github+json" \
//                 https://api.github.com/repos/gantla-pavan/catalogue-01/dependabot/alerts \
//                 > dependabot.json
//                 '''

//                 def alerts = readJSON file: 'dependabot.json'

//                 if (!alerts || alerts.size() == 0) {
//                     echo "✅ No Dependabot alerts found."
//                     return
//                 }

//                 def highAlerts = alerts.findAll {
//                     it.security_advisory?.severity in ['high','critical']
//                 }

//                 if (highAlerts.size() > 0) {

//                     echo "🚨 High/Critical vulnerabilities detected!"

//                     highAlerts.each {
//                         echo "Package: ${it.dependency.package.name}"
//                         echo "Severity: ${it.security_advisory.severity}"
//                         echo "Summary: ${it.security_advisory.summary}"
//                     }

//                     error("Build failed due to high/critical vulnerabilities")

//                 } else {
//                     echo "✅ No high/critical vulnerabilities"
//                 }
//             }
//         }
//     }
// }

//         stage('Build & Push Image') {
//             steps {
//                 withAWS(region: 'us-east-1', credentials: 'aws-credentials') {
//                     sh '''
//                     aws ecr get-login-password --region us-east-1 | \
//                     docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com

//                     docker build -t ${PROJECT}/${COMPONENT}:latest .

//                     docker tag ${PROJECT}/${COMPONENT}:latest \
//                     ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${APP_VERSION}

//                     docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${APP_VERSION}

//                     docker images
//                     '''
//                 }
//             }
//         }

//         stage('Test') {
//             steps {
//                 echo "Running tests..."
//             }
//         }

//         stage('Deploy') {
//             steps {
//                 echo "Deploy stage..."
//             }
//         }

//     }

//     post {
//         always {
//             echo 'Cleaning workspace...'
//             cleanWs()
//         }

//         success {
//             echo 'Pipeline succeeded'
//         }

//         failure {
//             echo 'Pipeline failed'
//         }

//         aborted {
//             echo 'Pipeline aborted'
//         }
//     }
// }





pipeline {
    agent {
        label 'AGENT-1'
    }

    environment {
        ACC_ID    = "690509490317"
        PROJECT   = "roboshop"
        COMPONENT = "catalogue"
        SONAR_AUTH_TOKEN = credentials('sonar-token')
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
                    env.APP_VERSION = packageJson.version
                    echo "App version is: ${env.APP_VERSION}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Dependabot Security Gate') {
    steps {
        withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
            script {

                sh '''
                echo "Fetching Dependabot alerts..."

                curl -s \
                -H "Authorization: Bearer $GITHUB_TOKEN" \
                -H "Accept: application/vnd.github+json" \
                https://api.github.com/repos/gantla-pavan/catalogue-01/dependabot/alerts \
                -o dependabot.json
                '''

                def alerts = readJSON file: 'dependabot.json'

                if (!(alerts instanceof List) || alerts.size() == 0) {
                    echo "✅ No Dependabot alerts found."
                    return
                }

                def highAlerts = []

                for (alert in alerts) {

                    if (alert.containsKey('security_advisory')) {

                        def severity = alert.security_advisory.severity

                        if (severity == 'high' || severity == 'critical') {
                            highAlerts.add(alert)
                        }
                    }
                }

                if (highAlerts.size() > 0) {

                    echo "🚨 High/Critical vulnerabilities detected!"

                    for (a in highAlerts) {
                        echo "Package: ${a.dependency.package.name}"
                        echo "Severity: ${a.security_advisory.severity}"
                        echo "Summary: ${a.security_advisory.summary}"
                    }

                    error("Build failed due to high/critical vulnerabilities")

                } else {
                    echo "✅ No high/critical vulnerabilities"
                }

            }
        }
    }
}
        stage('Build & Push Image') {
            steps {
                withAWS(region: 'us-east-1', credentials: 'aws-credentials') {
                    sh '''
                    aws ecr get-login-password --region us-east-1 | \
                    docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com

                    docker build -t ${PROJECT}/${COMPONENT}:latest .

                    docker tag ${PROJECT}/${COMPONENT}:latest \
                    ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${APP_VERSION}

                    docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${APP_VERSION}

                    docker images
                    '''
                }
            }
        }

        

        stage('Test') {
            steps {
                echo "Running tests..."
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploy stage..."
            }
        }

    }

    post {
        always {
            echo 'Cleaning workspace...'
            cleanWs()
        }

        success {
            echo 'Pipeline succeeded'
        }

        failure {
            echo 'Pipeline failed'
        }

        aborted {
            echo 'Pipeline aborted'
        }
    }
}