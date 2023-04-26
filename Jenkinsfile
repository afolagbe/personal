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
        nexuslogin = 'nuxuslogin'
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
        stage('CHECKSTYLE ANALYSIS'){
            steps{
                sh 'mvn -s settings.xml checkstyle'
            }
        }
    }
}