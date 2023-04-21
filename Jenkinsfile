pipeline{
    agent any
    tools{
        maven 'Maven'
        jdk 'JDK'
    }
    environment{
        SNAPREPO = 'vpro-snapshot'
        nexusgroup = 'vpro-maven-group'
        nexususer = 'admin'
        nexuspassword = 'admin'
        centralrepo = 'vpro-maven-central'
        nexusip = '172.31.0.191'
        nexusport = '8081'
        nexuslogin = 'nexuslogin'
        releaserepo = 'vpro-release'
    }
    stages{
        stage('BUILD'){
            steps{
                sh 'mvn -s settings.xml install -DskipTests'
            }
            post {
              success {
                    echo 'Now archiving'
                    archiveArtifacts:'**/*.war'
                }
            }
        }
        stage('TEST'){
            steps{
                sh 'mvn -s settings.xml test'
            }
        }
    }
}