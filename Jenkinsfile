pipeline {
    agent any

    environment {
        APP_NAME = 'SpringDataRest'
        DEPLOY_DIR = '/opt/tomcat/webapps/' // Change path if needed
    }

    options {
        skipDefaultCheckout(true)
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                checkout([$class: 'GitSCM', 
                    branches: [[name: 'main']], 
                    userRemoteConfigs: [[url: 'https://github.com/BOUSSELHAMYIYKI/project-SpringDataRest.git']]
                ])
            }
        }

        stage('Build') {
            steps {
                echo "Building project with Maven..."
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying WAR to Tomcat..."
                sh "cp target/*.war ${DEPLOY_DIR}"
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            echo 'Cleaning workspace...'
            cleanWs()
        }
    }
}
