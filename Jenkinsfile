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
                echo 'testing... by ${BUILD_USER}'
            }
        }
        stage('Approval') {
            // no agent, so executors are not used up when waiting for approvals
            agent none
            steps {
                script {
                    def deploymentDelay = input id: 'Deploy', message: 'Deploy to production?', submitter: 'rkivisto,admin', parameters: [choice(choices: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23', '24'], description: 'Hours to delay deployment?', name: 'deploymentDelay')]
                    sleep time: deploymentDelay.toInteger(), unit: 'HOURS'
                }
            }
        }
        stage('Deploy') {
            agent any
            steps {
                // uses https://plugins.jenkins.io/lockable-resources
                lock(resource: 'deployApplication'){
                    echo 'Deploying... by ${BUILD_USER}'
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
