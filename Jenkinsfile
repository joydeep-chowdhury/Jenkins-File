
pipeline {
    agent none
    environment {
        EMAIL_TO = 'joydeepc101@gmail.com;www.joycool@gmail.com'
    }
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
                echo 'testing... by '+getBuildUser()
                echo env.JOB_URL
            }
        }
        stage('Approval') {
            // no agent, so executors are not used up when waiting for approvals
            agent any
            steps {
                script {
                 def builduser
                 wrap([$class: 'BuildUser'])
                    {
                        builduser = "${BUILD_USER}"
                    }
                 echo builduser
              timeout(time: 10, unit: 'MINUTES') {
                
                   approvalMap = input (
                        id: 'userInput', 
                        message: "Approve build started by "+builduser,
                        ok: 'Approve',
                        submitter: '',
                        submitterParameter: 'APPROVER',
                        parameters: [[
                            $class: 'StringParameterDefinition', 
                            defaultValue: '30',
                            description: 'extend validity by N days', 
                            name: 'DAYS_TO_EXTEND']]
                        )
                        def adddays = "${approvalMap['DAYS_TO_EXTEND']}".toInteger()
                        def approver_name = "${approvalMap['APPROVER']}".toUpperCase()
                        echo  "Add days "+adddays+" Approver name: "+approver_name
                       }
                  
                  
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
