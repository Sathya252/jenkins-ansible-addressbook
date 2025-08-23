pipeline {
    agent any

    environment {
        // Skip Git host key checking temporarily
        GIT_SSH_COMMAND = "ssh -o StrictHostKeyChecking=no"
    }

    stages {

        stage('Clone AddressBook Project Repo') {
            steps {
                echo "Cloning AddressBook project repo..."
                git branch: 'main',
                    credentialsId: 'ansible-ssh',   
                    url: 'git@github.com:Sathya252/addressbook-cicd-project.git'
            }
        }

        stage('Build with Maven') {
            steps {
                // Go into the folder containing the pom.xml
                dir('addressbook-cicd-project') {
                    echo "Building project with Maven..."
                    sh 'mvn clean package'
                }
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
                    playbook: 'playbook.yml',
                    inventory: 'inventory.ini',
                    credentialsId: 'ansible-ssh',  // SSH credentials for target hosts
                    colorized: true,
                    disableHostKeyChecking: true,
                    installation: 'ansible2'
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
