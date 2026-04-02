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
                sh 'packer plugins install github.com/hashicorp/amazon'
                sh 'packer validate --var-file packer-vars.json packer.json'
            }
        }
    }
}