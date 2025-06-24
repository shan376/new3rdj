pipeline {
    agent any
    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
        SSH_DIR = "${env.WORKSPACE}/.ssh"
    }
    stages {
        stage('Clone Repo') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/shan376/new3rdj.git',
                        credentialsId: '15f23b05-1a56-496c-b8a1-3022db08a6a6'
                    ]]
                ])
            }
        }

        stage('Run Ansible') {
            steps {
                withCredentials([file(credentialsId: 'ansible_key', variable: 'PEM_FILE')]) {
                    sh '''
                        mkdir -p $SSH_DIR
                        chmod 700 $SSH_DIR

                        cp $PEM_FILE $SSH_DIR/app-server.pem
                        chmod 600 $SSH_DIR/app-server.pem

                        ssh-keyscan -H 3.144.102.81 >> $SSH_DIR/known_hosts

                        ansible-playbook -i inventory.ini playbook.yml \
                          --private-key=$SSH_DIR/app-server.pem
                    '''
                }
            }
        }
    }
}
