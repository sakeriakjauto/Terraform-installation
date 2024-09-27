pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Pull the code from your repository
                git branch: 'main', url: 'https://github.com/sakeriakjauto/Terraform-installation.git'
            }
        }

        stage('Set Up Python and Install Ansible') {
            steps {
                // Set up Python and install ansible-lint and Ansible
                sh '''
                    python3 -m pip install --upgrade pip
                    pip install ansible ansible-lint
                '''
            }
        }

        stage('Lint Ansible Playbooks') {
            steps {
                // Lint all YAML files
                sh 'ansible-lint **/*.yml'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // Run the Ansible playbook
                sh 'ansible-playbook install_terraform.yml'
            }
        }
    }

    post {
        always {
            // Cleanup or notifications
            echo 'Pipeline finished.'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
