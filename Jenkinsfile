node {
    checkout scm

    String jdktool = tool name: "JDK 1.8", type: 'hudson.model.JDK'
    def mvnHome = tool name: 'MAVEN'

    /* Set JAVA_HOME, and special PATH variables. */
    List javaEnv = [
        "PATH+MVN=${jdktool}/bin:${mvnHome}/bin",
        "M2_HOME=${mvnHome}",
        "JAVA_HOME=${jdktool}"
    ]

    withEnv(javaEnv) {
    stage ('Initialize') {
        sh '''
            echo "PATH = ${PATH}"
            echo "M2_HOME = ${M2_HOME}"
        '''
    }
    stage ('Build') {
        try {
            sh 'mvn -Dmaven.test.failure.ignore=true install'
        } catch (e) {
            currentBuild.result = 'FAILURE'
        }
    }
    stage ('Post') {
        if (currentBuild.result == null || currentBuild.result == 'SUCCESS') {
            junit 'target/surefire-reports/**/*.xml'  
        }
    }
  }
}
