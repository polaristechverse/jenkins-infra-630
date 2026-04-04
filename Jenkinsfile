@Library('my-infra-lib') _
pipeline {
    agent {
        label 'Dev'
    }
    parameters {
        choice(name: 'PACKER_BUILD', choices: ['no', 'yes'], description: 'Choose an action')
        choice(name: 'TERRAFORM_APPLY', choices: ['no', 'yes'], description: 'Choose an action')
        choice(name: 'TERRAFORM_DESTROY', choices: ['no', 'yes'], description: 'Choose an action')
        string(name: 'REGION', defaultValue: 'ap-south-1')
        choice(name: 'Ansible_Install', choices: ['no', 'yes'], description: 'Choose an action')
    }
    stages{
        stage('checking the software'){
            steps{
                sh '''
                terraform version
                packer version
                docker ps 
                ansible --version
                '''
            }
        }
        stage('packer build'){
                when {
                    expression { return params.PACKER_BUILD =='yes'}
                }
            steps{
                packerbuild()
            }
        }
        stage('capture amiid'){
            when{
                expression { return params.PACKER_BUILD == 'yes' }
            }
            steps{
                amicapture()
            }
        }
        stage('capture the latest ami'){
            steps {
                latestami(params.REGION)
            }
        }
        stage('Terraform_Plan') {
            steps{
                terraformplan()
            }
        }
        stage('Terraform_Apply'){
                when{
                expression { return params.TERRAFORM_APPLY == 'yes' }
            }
            steps{
                sh 'terraform apply --auto-approve'
            }
        }
        stage('Terraform_Destory'){
            when{
                expression { return params.TERRAFORM_DESTROY == 'yes '}
            }
            steps {
                sh 'terraform destory --auto-approve'
            }
        }
        stage('Ansible_Build'){
                    when {
                    expression { return params.Ansible_Install =='yes'}
                }
                steps {
                    sh 'ansible -i inventory/hosts.ini all -m ping'
                }
        }
    }
}