pipeline {
    agent {
        label 'AGENT-1'
    }

    environment {
        ACC_ID    = "515497299016"
        PROJECT   = "roboshop"
        COMPONENT = "catalogue"
        SONAR_AUTH_TOKEN = credentials('sonarcloud-token')
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

//         stage('Sonar Scan') {
//             steps {
//                 withSonarQubeEnv('SonarCloud') {   // Must match Jenkins Sonar config name
//                     sh '''
//                         npx sonar-scanner \
//                         -Dsonar.projectKey=gantla-pavan_catalogue-01 \
//                         -Dsonar.organization=gantla-pavan \
//                         -Dsonar.sources=. \
//                         -Dsonar.host.url=https://sonarcloud.io \
//                         -Dsonar.token=$SONAR_AUTH_TOKEN
//                     '''
//                 }
//             }
//         }

//        stage('Quality Gate') {
//     steps {
//         timeout(time: 1, unit: 'MINUTES') {
//             script {
//                 // Wait for SonarCloud/SonarQube Quality Gate result
//                 def qg = waitForQualityGate()
                
//                 if (qg.status != 'OK') {
//                     error "Pipeline aborted due to Quality Gate failure: ${qg.status}"
//                 } else {
//                     echo "Quality Gate passed: ${qg.status}"
//                 }
//             }
//         }
//     }
// }
stage('Dependabot Security Gate') {
            environment {
                GITHUB_OWNER = 'gantla-pavan'
                GITHUB_REPO  = 'catalogue-01'
                GITHUB_API   = 'https://api.github.com'
                GITHUB_TOKEN = 'https://api.github.com/repos/gantla-pavan/catalogue-01/dependabot/alerts'
            }
            steps {
                script {
                    // Fetch Dependabot alerts from GitHub API
                    def response = sh(
                        script: """
                        echo "Checking Dependabot alerts for ${GITHUB_OWNER}/${GITHUB_REPO}..."

                        response=\$(curl -s \\
                            -H "Authorization: Bearer ${GITHUB_TOKEN}" \\
                            -H "Accept: application/vnd.github+json" \\
                            -H "X-GitHub-Api-Version: 2022-11-28" \\
                            https://api.github.com/repos/${GITHUB_OWNER}/${GITHUB_REPO}/dependabot/alerts)

                        echo \$response
                        echo \$response > dependabot.json
                        """,
                        returnStdout: true
                    ).trim()

                    // Parse JSON using Jenkins readJSON
                    def alerts = readJSON file: 'dependabot.json'

                    // Filter high or critical severity alerts
                    def highAlerts = alerts.findAll { alert ->
                        alert.security_advisory.severity in ['high', 'critical']
                    }

                    if (highAlerts.size() > 0) {
                        echo "🚨 High/Critical Dependabot alerts found:"
                        highAlerts.each { a ->
                            echo "- Package: ${a.dependency.package.name}, Severity: ${a.security_advisory.severity}, Summary: ${a.security_advisory.summary}"
                        }
                        error "Build failed due to high/critical vulnerabilities!"
                    } else {
                        echo "✅ No High/Critical Dependabot alerts found."
                    }
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
            echo 'I will always say Hello again!'
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