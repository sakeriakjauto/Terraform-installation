pipeline {
    agent any  // This tells Jenkins to use any available agent to run the pipeline

    stages {
        stage('Checkout Code') {
            steps {
                // Step 1: Checkout the code from the repository
                git branch: 'main', url: 'https://github.com/sakeriakjauto/Terraform-installation.git'
            }
        }

        stage('Set Up Python Environment') {
            steps {
                // Step 2: Set up Python and install ansible-lint
                sh '''
                    python3 -m pip install --upgrade pip
                    pip install ansible ansible-lint
                '''
            }
        }

        stage('Lint All YAML Files') {
            steps {
                // Step 3: Lint all YAML files in the repository
                sh '''
                    find . -name "*.yml" -print0 | xargs -0 ansible-lint
                '''
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // Step 4: Run the main Ansible playbook
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
