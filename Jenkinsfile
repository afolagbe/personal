pipeline{
    agent any
    tools{
        jdk 'JDK'
        maven 'Maven'
    }
    environment{
        SNAPREPO = 'vpro-snapshot'
        RELEASEREPO = 'vpro-release'
        centralrepo = 'vpro-maven-central'
        nexusgroup = 'vpro-maven-group'
        nexususer = 'admin'
        nexuspassword = 'admin'
        nexusip = '172.31.0.191'
        nexusport = '8081'
        nexuslogin = 'nexuslogin'
    }
    stages{
        stage('BUILD'){
            steps{
                sh 'mvn -s settings.xml install -DskipTests'
            }
        }
    }
}