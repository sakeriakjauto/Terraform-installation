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

        stage('Install Python venv') {
            steps {
                // Step 2: Check and remove any existing apt locks before updating and installing python3-venv
                sh '''
                sudo rm -f /var/lib/apt/lists/lock
                sudo rm -f /var/cache/apt/archives/lock
                sudo apt-get update
                sudo apt-get install -y python3-venv
                '''
            }
        }

        stage('Set Up Python') {
            steps {
                // Step 3: Set up Python virtual environment
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install ansible ansible-lint
                '''
            }
        }

        stage('Lint install_terraform.yml') {
            steps {
                // Lint only the 'install_terraform.yml' file
                sh '''
                . venv/bin/activate
                ansible-lint install_terraform.yml
                '''
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // Step 5: Run the Ansible playbook
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
