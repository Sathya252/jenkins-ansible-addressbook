pipeline {
    agent any

    environment {
        // Force Git to skip host key verification temporarily (optional safety)
        GIT_SSH_COMMAND = "ssh -o StrictHostKeyChecking=no"
    }

    stages {
        stage('Clone Project Repo') {
            steps {
                echo "Cloning AddressBook project repo..."
                git branch: 'main',
                    credentialsId: 'ansible-ssh',   // SSH credential you created in Jenkins
                    url: 'git@github.com:Sathya252/addressbook-cicd-project.git'
            }
        }

        stage('Build with Maven') {
            steps {
                echo "Building project with Maven..."
                dir('addressbook-cicd-project') {   // Run Maven in the folder containing pom.xml
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
                    credentialsId: 'ansible-ssh',       // SSH credentials for target hosts
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
