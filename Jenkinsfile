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
                        credentialsId: 'cc2f35d9-7f84-4a2c-8552-7ddce001f59e'
                    ]]
                ])
            }
        }

        stage('Run Ansible') {
            steps {
                sh '''
                    mkdir -p $SSH_DIR
                    chmod 700 $SSH_DIR

                    # Copy the PEM key to workspace's SSH folder
                    cp /home/ubuntu/1stdep.pem $SSH_DIR/app-server.pem
                    chmod 600 $SSH_DIR/app-server.pem

                    # Generate known_hosts to avoid prompt
                    ssh-keyscan -H 3.144.102.81 >> $SSH_DIR/known_hosts

                    # Run Ansible playbook using inventory and key
                    ansible-playbook -i inventory.ini playbook.yml \
                      --private-key=$SSH_DIR/app-server.pem
                '''
            }
        }
    }
}
