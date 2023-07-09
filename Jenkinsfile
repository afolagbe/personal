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
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage ('Sonar Analysis') {
            steps {
                sh 'mvn clean verify sonar:sonar'
                sh '-Dsonar.projectKey=Project1 -Dsonar.host.url=http://54.234.2.29:9000 -Dsonar.login=sqp_c8573d48a96e4a483c8bef9d8ab33a927fcfd05e'
                    '-Dsonar.java.binaries='
                    
            }
        }
    }
}