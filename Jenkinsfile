pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('Kaviraj1994AccessKey')
        AWS_SECRET_ACCESS_KEY = credentials('Kaviraj1994SecretAccessKey')
    }

    parameters {
        choice(name: 'TerraformAction', choices: ['apply', 'destroy'], description: 'Select Terraform action')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'kravalji_Gitea_aakash', url: 'http://192.168.0.246:3000/kravalji/terraform_automation.git']])
            }
        }
        stage("terraform init") {
            steps {
                script {
                    def terraformPath = 'C:\\terraform_1.5.7_windows_386'
                    def workspacePath = 'D:\\Jenkins\\.jenkins\\workspace\\terraform_EC2_Instance'
                    
                    dir(workspacePath) {
                        // Change directory to your EC2_Instance folder
                        dir('CreateEC2InstancewithElasticIP_VPC_SG') {
                            bat "${terraformPath}\\terraform init -reconfigure"
                        }
                    }
                }
            }
        }
        stage("terraform plan") {
            steps {
                script {
                    def terraformPath = 'C:\\terraform_1.5.7_windows_386'
                    def workspacePath = 'D:\\Jenkins\\.jenkins\\workspace\\terraform_EC2_Instance'
                    
                    dir(workspacePath) {
                        // Change directory to your EC2_Instance folder
                        dir('CreateEC2InstancewithElasticIP_VPC_SG') {
                            bat "${terraformPath}\\terraform plan"
                        }
                    }
                }
            }
        }
        stage("terraform Action") {
            steps {
                script {
                    def terraformPath = 'C:\\terraform_1.5.7_windows_386'
                    def workspacePath = 'D:\\Jenkins\\.jenkins\\workspace\\terraform_EC2_Instance'
                    def action = params.TerraformAction
                    echo "Terraform action is --> ${action}"
                    
                    dir(workspacePath) {
                        // Change directory to your EC2_Instance folder
                        dir('CreateEC2InstancewithElasticIP_VPC_SG') {
                            if (action == 'apply') {
                                bat "${terraformPath}\\terraform apply --auto-approve"
                            } else if (action == 'destroy') {
                                bat "${terraformPath}\\terraform destroy --auto-approve"
                            } else {
                                error "Invalid Terraform action selected"
                            }
                        }
                    }
                }
            }
        }
    }
}
