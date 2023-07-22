def COLOR_MAP = [
    'SUCCESS' : 'good',
    'FAILURE' : 'danger'
]

pipeline{
    agent any
    tools {
        jdk 'JDK'
        maven 'maven'
    }
    environment {
        SONAR_SCANNER = 'SonarQube Scanner'
        SONAR_SERVER = 'SonarQube Server'
    }
    stages {
        stage ('PULL THE APPLICATION FROM GITHUB') {
            steps {
                git branch: 'ci-jenkins', url: 'https://github.com/afolagbe/personal.git'
            }
        }
        stage ('BUILD THE APPLICATION') {
            steps {
                sh 'mvn install -DeskipTest'
            }
        }
        stage ('TEST') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('UNIT TEST') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('INTEGRATION TEST') {
            steps {
                sh 'mvn verify -DskipUnitTest'
            }
        }
        stage ('CHECKSTYLE ANALYSIS') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
    }
    post {
        always{
            echo 'slack notifications'
            slackSend channel: '#project',
            color:COLOR_MAP[currentBuild.currentResult],
            message:"*${currentBuild.currentResult}:*project ${evn.PROJECT_NAME} build ${evn.BUILD_NUMBER}\n More info at ${BUILD_URL}"
        }
    }
}