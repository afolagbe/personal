pipeline{
    agent any
    tools {
        jdk 'JDK'
        maven 'Maven'
    }
    stages{
        stage('BUILD'){
            steps{
                sh 'mvn -s settingls.xml install -DeskipTests'
            }
        }
    }
}