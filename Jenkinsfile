def COLOR_MAP = [
    'SUCCESS':'good',
    'FAILURE':'danger'
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
        stage ('SLACK NOTIFICATION TO THE TEAM') {
            steps {
                echo 'Pipeline started'
            }
            post {
                always{
                    slackSend channel: '#ci-project',
                    color: 'warning'
                    message: 'build is started job name'
                }
            }
        }
        stage ('BUILD THE APPLICATION') {
            steps {
                sh 'mvn install -DeskipTest'
            }
        }
        stage ('TEST') {
            steps {
                sh ' mvn test'
            }
            post{
                success {
                    slackSend channel: '#ci-project',
                    color:'good',
                    message: "TEST IS SUCCESS"
                }
                failure {
                    slackSend channel: '#ci-project',
                    color: 'danger',
                    message: "TEST IS FAILED"
                }
            }
        }
        stage ('UNIT TEST') {
            steps {
                sh 'mvn test'
            }
            post{
                success {
                    slackSend channel: '#ci-project',
                    color:'good',
                    message: "UNIT TEST IS SUCCESSFUL"
                }
                failure {
                    slackSend channel: '#ci-project',
                    color:'danger',
                    message: "UNIT TEST IS FAILED"
                }
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
            slackSend channel: '#ci-project',
            color: COLOR_MAP[currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job name ${env.JOB_NAME} build ${env.BUILD_NUMBER} time ${env.BUILD_TIMESTAMP} \n More info at: ${BUILD_URL}"
        }
    }
}