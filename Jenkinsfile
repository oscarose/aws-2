pipeline {
    agent {
        label'master'
    }
    parameters {
        choice(name:'aws_region', choices: ['us-east-1', 'us-west-2'], description: 'aws region to deploy',)
        string(name: 'stack_name', defaultValue: '', description: 'name of cft stack',)
        choice(name:'state', choices: ['present', 'absent'], description: 'cft build or teardown condition',)
    }
    stages {
         stage('checkout scm') {
             steps{
                 git branch: 'master',
                     credentialsId: 'Github',
                         url: 'https://github.com/oscarose/aws-2.git'
             }
         }
         stage('deploy cloudformation template') {
             steps{
                 withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-2', accessKeyVariable: 'aws_access_key', secretKeyVariable: 'aws_secret_key']]){
                     script {
                          if(aws_region=="us-east-1") {
                              sh """
                              echo "deploying us-east-1 stack"
                              ansible-playbook  --extra-vars "ansible_python_interpreter=/usr/bin/python stack_name=${stack_name} state=${state} aws_region=${aws_region} aws_secret_key=$aws_secret_key aws_access_key=$aws_access_key" ${WORKSPACE}/playbook.yaml
                              echo "stack creation is complete"
                              """
                          }
                          if(aws_region=="us-west-2") {
                              sh 'ansible-playbook --extra-vars "ansible_python_interpreter=/usr/bin/python stack_name=${stack_name} state=${state} aws_region=${aws_region} aws_secret_key=${aws_secret_key} aws_access_key=${aws_access_key}" ${WORKSPACE}/playbook.yaml'
                          }
                     }
                 }
             }
         }
    }
}
