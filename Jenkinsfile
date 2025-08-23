pipeline {
    agent any

    environment {
        // Skip Git host key verification (optional safety)
        GIT_SSH_COMMAND = "ssh -o StrictHostKeyChecking=no"
    }

    stages {
        stage('Clone Project Repo') {
            steps {
                echo "Cloning AddressBook project repo..."
                git branch: 'main',
                    credentialsId: 'ansible-ssh',  // SSH credential for Git
                    url: 'git@github.com:Sathya252/addressbook-cicd-project.git'
            }
        }

        stage('Build with Maven (Optional)') {
            steps {
                echo "Building project with Maven (optional)..."
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
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
                    credentialsId: 'ansible-ssh',       // SSH credentials for node
                    colorized: true,
                    disableHostKeyChecking: true,
                    installation: 'ansible2'
                )
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully! AddressBook should be deployed."
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}
