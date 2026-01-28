pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SurendraImmanni/testproject.git'
            }
        }

        stage('Install Ansible on Jenkins') {
            steps {
                sh '''
                if ! sudo -u azureuser command -v ansible >/dev/null 2>&1; then
                  sudo -u azureuser sudo apt update
                  sudo -u azureuser sudo apt install -y ansible
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
