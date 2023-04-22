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
    nexusip = '172.31.10.98'
    nexusport = '8081'
    nexusgroup = 'vpro-maven-group'
    nexuslogin = 'nexuslogin'
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
    }
}