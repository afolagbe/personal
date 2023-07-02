pipeline{
    agent any
    tools {
        jdk 'JDK'
        maven "maven"
    }
    stages {
        stage ('BUILD THE APPLICATION'){
            steps {
                sh 'mvn install -DeskipTests'
            }
        }
    }
}