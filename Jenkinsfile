@Library('my-infra-lib') _
pipeline {
    agent {
        label 'Dev'
    }
    parameters {
        choice(name: 'PACKER_BUILD', choices: ['no', 'yes'], description: 'Choose an action')
        choice(name: 'TERRAFORM_APPLY', choices: ['no', 'yes'], description: 'Choose an action')
        choice(name: 'TERRAFORM_DESTROY', choices: ['no', 'yes'], description: 'Choose an action')
    }
    stages{
        stage('checking the software'){
            steps{
                sh '''
                terraform version
                packer version
                docker ps 
                '''
            }
        }
        stage('packer build'){
                when {
                    expression { return params.PACKER_BUILD =='yes'}
                }
            steps{
                packerBuild()
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
                latestami()
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
    }
}