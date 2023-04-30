pipeline{
    agent any
    tools{
        jdk 'myjdk'
        maven 'Maven'
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
            post{
                success{
                    echo 'now archiving'
                    archiveArtifacts artifacts: '**/*.war', followSymlinks: false
                }
            }
        }
        stage('TEST'){
            steps{
                sh 'mvn -s settings.xml test'
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
    }
}