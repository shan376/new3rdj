pipeline {
    agent any
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
                withEnv(["ANSIBLE_HOST_KEY_CHECKING=False"]) {
                    sh '''
                    mkdir -p /var/lib/jenkins/.ssh
                    chmod 700 /var/lib/jenkins/.ssh
                    ssh-keyscan -H 3.144.102.81 >> /var/lib/jenkins/.ssh/known_hosts
                    ansible-playbook -i inventory.ini playbook.yml
                    '''
                }
            }
        }
    }
}
