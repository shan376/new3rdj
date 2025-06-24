pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = "False"
        ANSIBLE_PRIVATE_KEY_FILE = "/var/lib/jenkins/.ssh/1stdep.pem"
    }

    stages {
        stage('Clone Repo') {
            steps {
                checkout([
                    $class: 'GitSCM',
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
                # Add target server to known_hosts
                ssh-keyscan -H 3.144.102.81 >> /var/lib/jenkins/.ssh/known_hosts

                # Run ansible playbook with key
                ansible-playbook -i inventory.ini playbook.yml --private-key=$ANSIBLE_PRIVATE_KEY_FILE
                '''
            }
        }
    }
}
