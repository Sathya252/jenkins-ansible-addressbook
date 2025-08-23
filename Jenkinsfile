pipeline {
    agent any

    environment {
        GIT_SSH_COMMAND = "ssh -o StrictHostKeyChecking=no"
    }

    stages {
        stage('Clone Project Repo') {
            steps {
                echo "Cloning AddressBook project repo..."
                git branch: 'main',
                    credentialsId: 'ansible-ssh',
                    url: 'git@github.com:Sathya252/addressbook-cicd-project.git'
            }
        }

        stage('Build with Maven') {
            steps {
                echo "Building project with Maven..."
                sh 'mvn clean package'
            }
        }

        stage('Checkout Playbook Repo') {
            steps {
                echo "Cloning Jenkins-Ansible repo for playbooks..."
                git branch: 'main',
                    credentialsId: 'ansible-ssh',
                    url: 'git@github.com:Sathya252/jenkins-ansible-addressbook.git'
            }
        }

        stage('Deploy with Ansible') {
            steps {
                echo "Running Ansible playbook..."
                ansiblePlaybook(
                    playbook: 'addressbook.yml',
                    inventory: 'inventory.ini',
                    credentialsId: 'ansible-ssh',
                    colorized: true,
                    disableHostKeyChecking: true,
                    installation: 'Ansible'
                )
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}
