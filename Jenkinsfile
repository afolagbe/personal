pipeline{
    agent any
    tools {
        jdk 'myjdk'
        maven 'Maven'
    }
    environment{
        SNAPREPO = 'vpro-snapshots'
        NEXUSUSER = 'admin' 
        NEXUSPASSWORD = 'admin'
        releaserepo = 'vpro-release'
        centralrepo = 'vpro-maven-central'
        nexusip = '172.31.10.98'
        nexusport = '8081'
        nexusgrouprepo = 'vpro-maven-group'
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