pipeline{
    agent any
    tools {
        jdk 'JDK'
        maven 'Maven'
    }
    stages{
        stage('BUILD'){
            steps{
                sh 'mvn -sa settings.xml install -DeskipTests'
            }
        }
    }
}