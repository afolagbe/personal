pipeline{
    agent any
    tools {
        jdk 'JDK'
        maven "Maven"
    }
    stages {
        stage ('BUILD THE APPLICATION'){
            steps {
                sh 'mvn install -DeskipTests'
            }
        }
    }
}