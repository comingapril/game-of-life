pipeline {
    agent { label 'MAVEN_JDK8' }
    triggers { pollSCM ( 'H/30 * * * *')}
    parameters {
        choice (name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'Maven Goal')
    }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/comingapril/game-of-life.git',
                    branch: 'declarative'
            }
        }
        stage('package') {
            tools {
                jdk 'JDK_8_UBUNTU'
            }
            steps {
                sh "mvn ${params.MAVEN_GOAL}"
                }
        }
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/gameoflife.war',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
    post {
        success {
            mail subject: "Jenkins build of ${JOB_NAME} with id ${BUILD_ID} is success",
                 body: "use this URL ${BUILD_URL} for more info",
                 to: 'team-all-@nav.com',
                 from: 'navi@qt.com'
        }
        failure {
             mail subject: "Jenkins build of ${JOB_NAME} with id ${BUILD_ID} is failed",
                 body: "use this URL ${BUILD_URL} for more info",
                 to: "${GIT_AUTHOR_EMAIL}",
                 from: 'navi@qt.com'
        }
    }
}
