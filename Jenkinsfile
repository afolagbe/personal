pipeline{
    agent any
    tools {
        jdk 'JDK'
        maven "Maven"
    }
    stages ('BUILD THE APPLICATION') {
        steps {
            sh 'mvn install DskipTests'
        }
    }
}