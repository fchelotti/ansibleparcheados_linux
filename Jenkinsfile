pipeline {
    agent {
        label "sul-ansible"
    }

    parameters {
        string(defaultValue: "10.106.245.16", name: "Server")
    }

    stages {

        stage('Ansible Clone Repo') {
            steps {
                container('ansible') {
                    git credentialsId: 'bitbucket_token', url: 'https://github.com/fchelotti/ansibleparcheados_linux.git', branch: 'master'
                }
            }
        }

        stage('Pre Build') {
            steps {
                container('ansible') {
                    sh 'for ip in ${Server}; do sed -i "/linux/a $ip" Modulos/hosts; done'
                }
            }
        }

        stage('Apply Parcheados') {
            steps {
                withCredentials([string(credentialsId: 'root_password', variable: 'root_passwd')
                ]){
                container('ansible') {
                    script {
                        sh 'ansible-playbook -i Modulos/hosts Modulos/parcheado-linux.yaml -e "root_password=${root_passwd}" -e "subscription=Brasil"'
                    }
                }
                }
            } 
        }
    }
}