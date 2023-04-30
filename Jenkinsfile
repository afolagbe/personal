pipeline{
    agent any
    tools {
        maven 'MAVEN3'
        jdk 'OracleJDK8'
    }
    environment {
        SNAP_REPO = 'vpro-snapshots'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = 'vpro-release'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUS_IP = '172.31.13.83'
        NEXUS_PORT = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
        SONAR_SERVER = 'SONARSERVER'
        SONAR_SCANNER = 'SONARSCANNER'
    }
    stages{
        stage('NOTIFICATION TO SLACK'){
            steps {
                echo 'Pipeline started'
            }
            post {
                always{
                    slackSend channel: '#devops-project',
                    color: 'good',
                    message: "Job is started Job name: ${env.JOB_NAME} build ${env.BUILD_NUMBER} time ${env.BUILD_TIMESTAMP} \n More info at: ${BUILD_URL}"
                }
            }
        }
        stage('BUILD'){
            steps{
                sh 'mvn -s settings.xml install -DskipTests'
            }
            post {
                success {
                    echo 'Now archiving'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('UNIT TEST'){
            steps{
                sh 'mvn -s settings.xml test'
            }
            post {
                success {
                    slackSend channel: '#devops-project',
                    color: 'good',
                    message: "UNIT TEST IS SUCCESS"
                }
                failure {
                    slackSend channel: '#devops-project',
                    color: 'danger',
                    message: "UNIT TEST IS FAILED"
                }
            }
        }
        stage('INTEGRATION TEST'){
            steps{
                sh 'mvn -s settings.xml verify -DskipUnitTests'
            }
        }
        stage('CHECKSTYLE ANALYSIS'){
            steps{
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }
        stage ('SONAR ANALYSIS') {
            environment {
                scannerHome = tool "${ SONAR_SCANNER}"
            }
            steps {
                withSonarQubeEnv("${SONAR_SERVER}") {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }
            }
        }
        stage('QUALITY GATE') {
            steps{
                timeout (time:1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        }
    }