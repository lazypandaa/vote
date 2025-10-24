pipeline {
    agent any
    
    tools {
        nodejs 'NodeJS'
        maven 'MAVEN_HOME'
        jdk 'JAVA_HOME'
    }

    stages {

        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('FRONTEND') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                sh '''
                mkdir -p ${WORKSPACE}/deploy/frontend
                cp -r FRONTEND/dist/* ${WORKSPACE}/deploy/frontend/
                echo "Frontend deployed to ${WORKSPACE}/deploy/frontend"
                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('BACKEND') {
                    sh 'mvn clean package'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                sh '''
                mkdir -p ${WORKSPACE}/deploy/backend
                cp BACKEND/target/*.war ${WORKSPACE}/deploy/backend/
                echo "Backend deployed to ${WORKSPACE}/deploy/backend"
                '''
            }
        }

    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed.'
        }
    }
}