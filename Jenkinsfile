pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Step 1: Checkout code from the repository
                checkout([$class: 'GitSCM', branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/sakeriakjauto/Terraform-installation.git']]
                ])
            }
        }

        stage('Set Up Python') {
            steps {
                // Step 2: Set up Python environment
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install ansible ansible-lint
                '''
            }
        }

        stage('Lint YAML Files') {
            steps {
                // Step 3: Lint all YAML files in the repository
                sh '''
                . venv/bin/activate
                find . -name "*.yml" -print0 | xargs -0 ansible-lint
                '''
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // Step 4: Run the Ansible playbook
                sh '''
                . venv/bin/activate
                ansible-playbook install_terraform.yml -vvv
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
