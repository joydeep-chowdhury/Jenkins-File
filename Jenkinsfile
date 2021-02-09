pipeline {
    agent none
    stages {
        stage('Build') {
            agent any
            steps {
                echo 'compiling... by '+getBuildUserId()
              
            }
        }
        stage('Test') {
            agent any
            steps {
                echo 'testing... by ${BUILD_USER} '
                echo env.BUILD_URL
            }
        }
        stage('Approval') {
            // no agent, so executors are not used up when waiting for approvals
            agent any
            steps {
                script {
              timeout(time: 10, unit: 'MINUTES') {
                input(id: "Deploy Gate", message: "Deploy ${params.project_name}?", ok: 'Deploy')
              }
            }
            }
        }
        stage('Deploy') {
            agent any
            steps {
                // uses https://plugins.jenkins.io/lockable-resources
                lock(resource: 'deployApplication'){
                    echo 'Deploying... by '+getBuildUserId()
                }
            }
        }
    }
}

def getBuildUserId()
{
    def jobUserId
    wrap([$class: 'BuildUser']) 
        {
        jobUserId = "${BUILD_USER_ID}"
        }
    return jobUserId
}
def getBuildEmail()
{
     def jobUserEmail
    wrap([$class: 'BuildUser']) 
        {
        jobUserEmail = "${BUILD_USER_EMAIL}"
        }
    return jobUserEmail
}
def getBuildUser()
{
     def jobUser
    wrap([$class: 'BuildUser']) 
        {
        jobUser = "${BUILD_USER}"
        }
    return jobUser
}
