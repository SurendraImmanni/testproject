pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-org/your-repo.git'
            }
        }

        stage('Install Ansible on Jenkins') {
            steps {
                sh '''
                if ! command -v ansible >/dev/null 2>&1; then
                  sudo apt update
                  sudo apt install -y ansible
                fi
                '''
            }
        }

        stage('Wait for Ansible') {
            steps {
                sh '''
                sleep 10
                ansible --version
                '''
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sshagent(credentials: ['ssh-key']) {
                    sh '''
                    ansible-playbook -i inventory.ini docker-creation.yml
                    '''
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
