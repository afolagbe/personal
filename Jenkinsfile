pipeline{
    agent any
    tools{
        jdk 'myjdk'
        maven 'Maven'
    }
    environment{
        SNAPREPO = 'vpro-snapshts'
        NEXUSUSER = 'admin'
        nexuspassword = 'admin'
        releaserepo = 'vpro-release'
        centralrepo = 'vpro-maven-central'
        nexusip = '172.31.10.98'
        nexusport = '8081'
        nexusgroup = 'vpro-maven-group'
        nexuslogin = 'nexuslogin'
    }
    stage{
        stage('BUILD'){
            steps{
                sh 'mvn -s settings.xml install -DskipTests'
            }
        }
    }
}