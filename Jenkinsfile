pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {

        stage('Checkout Ansible Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/priyabratakhandual/ansible-jenkins.git'
            }
        }

        stage('Validate Inventory') {
            steps {
                sh '''
                ansible-inventory -i inventory --list
                '''
            }
        }

        stage('Ping Target Servers') {
            steps {
                sh '''
                ansible web -i inventory -m ping
                '''
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sh '''
                ansible-playbook -i inventory nginx.yaml
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Ansible deployment completed successfully"
        }
        failure {
            echo "❌ Ansible deployment failed"
        }
    }
}
