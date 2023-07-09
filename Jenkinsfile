pipeline{
    agent any
    tools {
        jdk 'JDK'
        maven 'maven'
    }
    environment {
        SONAR_SERVER = 'SonarQube sever'
        SONAR_SCANNER = 'Sonarscan'
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
        stage ('CHECKSTYLE ANALYSIS') {
            steps {
                sh 'mvn checkstyle:checkstyl'
            }
        }
        stage ('SONAR ANALYSIS') {
            environment {
                scannerHome = tool "${SONAR_SCANNER}"
            }
            steps {
              sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=Project1 \
                   -Dsonar.projectName=Project1 \
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