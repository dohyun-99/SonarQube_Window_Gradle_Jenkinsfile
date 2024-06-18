pipeline {
    agent any
    
    environment {
        GRADLE_HOME = "C://Gradle//gradle-8.8"
        PATH = "${GRADLE_HOME}/bin:${env.PATH}"
        
        projectName = 'CI with Jenkins'
        projectKey = 'gradle-ci-test'
        sonarHostUrl = 'http://localhost:9000'
        sonarToken = credentials('SonarQubeToken')
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build & Test') {
            steps {
                script {
                    // Gradle을 사용하여 프로젝트 빌드 및 Jacoco 테스트 보고서 생성
                    sh "./gradlew clean build jacocoTestReport -Dsonar.host.url=${sonarHostUrl} -Dsonar.token=${sonarToken}"
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    // SonarQube 분석 실행
                    sh "gradlew.bat sonarqube -Dsonar.projectKey=${projectKey} -Dsonar.projectName=${projectName} -Dsonar.host.url=${sonarHostUrl} -Dsonar.login=${sonarToken}"
                }
            }
        }
    }
    
    post {
        success {
            echo "Pipeline completed successfully"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}