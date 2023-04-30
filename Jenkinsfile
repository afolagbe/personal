pipeline{
    agent any
    tools{
        jdk 'JDK'
        maven 'Maven'
    }
    environment{
        NEXUSUSER = 'admin'
        nexusgroup = 'vpro-maven-group'
        nexusip = '172.31.0.191'
        releaserepo = 'vpro-release'
        centralrepo = 'vpro-maven-central'
        nexusport = '8081'
        nexuslogin = 'nexuslogin'
        nexuspassword = 'admin'
        SNAPREPO = 'vpro-snapshot'
        SONAR_SERVER = 'Sonarserver'
        SONAR_SCANNER = 'Sonarscanner'
    }
    stages{
        stage('BUILD'){
            steps{
                sh 'mvn -s settings.xml install -DskipTests'
            }
            post{
                success {
                    echo 'Now archiving'
                    archiveArtifacts artifacts: 'target/*war*', followSymlinks: false
                }
            }
        }
        stage('TEST'){
            steps{
                sh 'mvn -s settings.xml test'
            }
        }
        stage('UNIT TEST'){
            steps{
                sh 'mvn -s settings.xml test'
            }
        }
        stage ('SONAR ANALYSIS') {
            environment {
                scannerHome = tool "${SONAR_SCANNER}"
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
        stage('QUALITY GATE'){
            steps{
                timeout (time:10, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}