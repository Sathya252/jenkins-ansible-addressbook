pipeline {
    agent any

    environment {
        // Skip SSH host key checking temporarily
        GIT_SSH_COMMAND = "ssh -o StrictHostKeyChecking=no"
    }

    stages {
        stage('Clone Maven Project Repo') {
            steps {
                echo "Cloning AddressBook project repo..."
                dir('addressbook-cicd-project') {
                    git branch: 'master',
                        credentialsId: 'ansible-ssh',
                        url: 'git@github.com:Sathya252/addressbook-cicd-project.git'
                }
            }
        }

        stage('Build with Maven') {
            steps {
                echo "Building project with Maven..."
                dir('addressbook-cicd-project') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Checkout Ansible Playbook Repo') {
            steps {
                echo "Cloning Jenkins-Ansible repo for playbooks..."
                dir('jenkins-ansible-addressbook') {
                    git branch: 'main',
                        credentialsId: 'ansible-ssh',
                        url: 'git@github.com:Sathya252/jenkins-ansible-addressbook.git'
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                echo "Running Ansible playbook..."
                dir('jenkins-ansible-addressbook') {
                    ansiblePlaybook(
                        playbook: 'playbook.yml',
                        inventory: 'inventory.ini',
                        credentialsId: 'ansible-ssh',
                        colorized: true,
                        disableHostKeyChecking: true,
                        installation: 'ansible2'
                    )
                }
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
