@Library('my-infra-lib') _
pipeline {
    agent {
        label 'Dev'
    }
    parameters {
        choice(name: 'PACKER_BUILD', choices: ['no', 'yes'], description: 'Choose an action')
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
    }
}