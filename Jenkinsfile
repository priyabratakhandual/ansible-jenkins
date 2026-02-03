pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {

        stage('Validate Inventory') {
            steps {
                sh '''
                echo "---- Inventory Check ----"
                ansible-inventory -i inventory --list
                '''
            }
        }

        stage('Ping Target Servers') {
            steps {
                sshagent(['ansible-ssh']) {
                    sh '''
                    echo "---- Ansible Ping Test ----"
                    ansible web -i inventory -m ping
                    '''
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sshagent(['ansible-ssh']) {
                    sh '''
                    echo "---- Running Ansible Playbook ----"
                    ansible-playbook -i inventory nginx.yml
                    '''
                }
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
