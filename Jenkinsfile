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
        stage ('TEST THE APPLICATION') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('UNIT TEST') {
            steps {
                sh 'mvn install -DeskipUnitTest'
            }
        }
        stage ('INTEGRATION TEST') {
            steps {
                sh 'mvn install -DeskipUnit Test'
            }
        }
    }
}