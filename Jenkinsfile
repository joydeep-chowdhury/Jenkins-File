
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
                emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                    to: "${EMAIL_TO}", 
                    subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
                 def builduser
                 wrap([$class: 'BuildUser'])
                    {
                        builduser = "${BUILD_USER}"
                    }
                 echo builduser
                    
                      approvalMap = input (
                        id: 'userInput', 
                        message: "Approve build started by "+builduser+" for job $JOB_NAME",
                        ok: 'Approve',
                        submitter: '',
                        submitterParameter: 'APPROVER'
                        )  
                      def approver_name="${approvalMap['APPROVER']}".toUpperCase()
                    
                    echo "Approver name"+approver_name             
                    
                    
                    
                    
                    
                    
                    
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
