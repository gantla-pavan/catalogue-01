pipeline {
    agent {
        label 'AGENT-1'
    }

    environment {
        COURSE           = "jenkins"
        ACC_ID           = "515497299016"
        PROJECT          = "roboshop"
        COMPONENT        = "catalogue"
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
                    env.appVersion = packageJson.version
                    echo "App version is: ${env.appVersion}"
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

        stage('Sonar Scan') {
            steps {
                script {
                    sh """
                        npx sonar-scanner \
                        -Dsonar.projectKey=gantla-pavan_catalogue-01 \
                        -Dsonar.organization=gantla-pavan \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=${SONAR_AUTH_TOKEN}
                    """
                }
            }
        }

        stage('Build & Push Image') {
            steps {
                withAWS(region: 'us-east-1', credentials: 'aws-credentials') {
                    sh """
                        aws ecr get-login-password --region us-east-1 | \
                        docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com

                        docker build -t ${PROJECT}/${COMPONENT}:latest .

                        docker tag ${PROJECT}/${COMPONENT}:latest \
                        ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${env.appVersion}

                        docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${env.appVersion}

                        docker images
                    """
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
            echo 'I will run if success'
        }
        failure {
            echo 'I will run if failure'
        }
        aborted {
            echo 'Pipeline is aborted'
        }
    }
}

