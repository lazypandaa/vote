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
                rm -rf /usr/local/Cellar/tomcat/*/libexec/webapps/2300032990-frontend
                mkdir -p /usr/local/Cellar/tomcat/*/libexec/webapps/2300032990-frontend
                cp -r FRONTEND/dist/* /usr/local/Cellar/tomcat/*/libexec/webapps/2300032990-frontend/
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
                rm -f /usr/local/Cellar/tomcat/*/libexec/webapps/2300032990-backend.war
                rm -rf /usr/local/Cellar/tomcat/*/libexec/webapps/2300032990-backend
                cp BACKEND/target/*.war /usr/local/Cellar/tomcat/*/libexec/webapps/
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