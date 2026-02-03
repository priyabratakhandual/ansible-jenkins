pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {

        stage('Validate Inventory') {
            steps {
                sh '''
                ansible-inventory -i inventory --list
                '''
            }
        }

        stage('Ping Target Servers') {
            steps {
                sshagent(['ansible-ssh']) {
                sh '''
                ansible web -i inventory -m ping
                '''
            }
       }    
   }
        stage('Run Ansible Playbook') {
            steps {
                sshagent(['ansible-ssh']) {
                sh '''
                ansible-playbook -i inventory playbook.yml
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
