pipeline {
    agent any
    
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to deploy')
        choice(name: 'DEPLOY_ENV', choices: ['development', 'staging', 'production'], description: 'Deployment Environment')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests before deploying?')
        text(name: 'CUSTOM_MESSAGE', defaultValue: '', description: 'Custom message for the deployment')
    }

    environment {
        DEPLOY_DIR = 'build'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from the Git repository using the passed BRANCH_NAME parameter
                    echo "Checking out branch: ${params.BRANCH_NAME}"
                    checkout scm: [
                        $class: 'GitSCM', 
                        branches: [[name: "*/${params.BRANCH_NAME}"]],
                        userRemoteConfigs: [[url: 'https://github.com/NithishNithi/MyTestServer.git']]
                    ]
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                // Add your build command (e.g., Maven, Gradle, npm)
                // sh 'mvn clean install'
            }
        }

        stage('Test') {
            when {
                expression {
                    return params.RUN_TESTS == true
                }
            }
            steps {
                echo 'Running unit tests...'
                // Run your test command (e.g., Maven, npm test)
                // sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the application...'
                // Package the application (e.g., Maven package)
                // sh 'mvn package'
            }
        }

        stage('Deploy') {
            when {
                expression {
                    return params.DEPLOY_ENV == 'production' || params.DEPLOY_ENV == 'staging'
                }
            }
            steps {
                echo "Deploying to the ${params.DEPLOY_ENV} environment..."
                // Deploy the application based on the selected environment
                // sh "./deploy.sh ${params.DEPLOY_ENV} ${params.CUSTOM_MESSAGE}"
            }
        }

        stage('Post-deployment tests') {
            when {
                expression {
                    return params.DEPLOY_ENV == 'production'
                }
            }
            steps {
                echo 'Running post-deployment tests...'
                // Run post-deployment tests (e.g., health check, smoke test)
                // sh 'curl http://your-app-url/healthcheck'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
        always {
            echo 'Cleaning up...'
        }
    }
}
