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
        nexusip = '172.31.10.98'
        nexusport = '8081'
        nexuslogin = 'nexuslogin'
        releaserepo = 'vpro-release'
    }
    stages{
              success {
                    echo 'Now archiving'
            archiveArtifacts artifacts: 'new', followSymlinks: false
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