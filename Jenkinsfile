pipeline {
    agent any
    
    environment {
        GRADLE_HOME = tool 'Gradle' // Jenkins에서 설정한 Gradle 도구
        PATH = "${GRADLE_HOME}/bin:${env.PATH}"
        
        // 본인 프로젝트 및 설정
        projectName = 'CI with Jenkins'
        projectKey = 'gradle-ci-test'
        sonarHostUrl = 'http://localhost:9000' // SonarQube 서버 URL
        sonarToken = credentials('sonar-token') // Jenkins에 설정된 SonarQube 토큰 Credential ID
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
                    sh "gradlew.bat clean build jacocoTestReport -Dsonar.host.url=${sonarHostUrl} -Dsonar.token=${sonarToken}"
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


def getTag(branchName){     
    def TAG
    def DATETIME_TAG = new Date().format('yyyyMMddHHmmss')
    TAG = "${DATETIME_TAG}"
    return TAG
}  
