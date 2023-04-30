def COLOR_MAP=[
    'SUCCESS':'good',
    'FAILURE':'danger'
]

pipeline{
    agent any
    tools {
        maven 'Maven'
        jdk 'JDK'
    }
    environment{
    SNAPREPO = 'vpro-snapshots'
    NEXUSUSER = 'admin'
    nexuspassword = 'admin'
    releaserepo = 'vpro-release'
    centralrepo = 'vpro-mavan-central'
    nexusip = '44.204.226.97'
    nexusport = '8081'
    nexusgroup = 'vpro-maven-group'
    nexuslogin = 'nexuslogin'
    SONARSERVER = 'Sonarserver'
    SONAR_SCANNER = 'Sonarscanner'
    }
    stages{
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
                scannerHome = tool "${SONAR_SCANNER}"
            }
            steps {
                withSonarQubeEnv("${SONARSERVER}") {
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
        stage('UPLOAD ARTIFACT TO NEXUS'){
            steps{
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusip}:${nexusport}",
                    groupId: 'SALES',
                    version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                    repository: "${releaserepo}",
                    credentialsId: "${nexuslogin}",
                    artifacts: [
                        [artifactId: 'vproapp',
                        classifier: '',
                        file: 'target/vprofile-v2.war',
                        type: 'war']
                    ]
                )
            }
        }
    }
    post {
        always {
            echo 'slack notifications'
            slackSend channel: '#project',
            color: COLOR_MAP[currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} time ${env.BUILD_TIMESTAMP} \n More info at: ${BUILD_URL}"
        }
        stages{
            stage(notifications to slack){
                steps{
                    echo 'Pipeline-started'
                }
                post{
                    always{
                        slackSend channel:'#project',
                        message:"Job is started"
                    }
                }
            }
        }
    }
}