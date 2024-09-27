pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/sakeriakjauto/Terraform-installation.git'
            }
        }

        stage('Set Up Python Environment') {
            steps {
                sh '''
                    python3 -m pip install --upgrade pip
                    pip install ansible ansible-lint
                '''
            }
        }

        stage('Install Ansible Collections') {
            steps {
                // Install any required Ansible collections if specified in requirements.yml
                sh 'ansible-galaxy collection install -r requirements.yml || true'
            }
        }

        stage('Lint All YAML Files') {
            steps {
                // Update find command to skip non-playbook YAML files
                sh '''
                    find . -name "*.yml" ! -path "./.github/*" -print0 | xargs -0 ansible-lint
                '''
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sh 'ansible-playbook install_terraform.yml'
            }
        }
    }

    post {
        always {
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
