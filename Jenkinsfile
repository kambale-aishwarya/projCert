pipeline {
    agent {label 'master'}
    stages {
        stage ('Deploy_JAVA_Chrome'){
            agent {label 'master'} 
            steps {
                git changelog: false, poll: false, url: 'https://github.com/kambale-aishwarya/projCert.git'
                ansiblePlaybook becomeUser: null, credentialsId: 'ubuntu_ansiblemaster', disableHostKeyChecking: true, installation: 'Ansible', playbook: 'edu_project.yml', sudoUser: null
            }
        }
        stage ('Deploy_DOCKER'){
            agent {label 'master'}
            steps{
          //     git changelog: false, poll: false, url: 'https://github.com/kambale-aishwarya/projCert.git'
               ansiblePlaybook becomeUser: null, credentialsId: 'ubuntu_ansiblemaster', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'docker_hosts', playbook: 'ed_proj_docker.yml', sudoUser: null
            }
        }
        stage ('Deploy_WEB'){
            agent {label 'slave-1'}
            steps{
            //  git changelog: false, poll: false, url: 'https://github.com/kambale-aishwarya/projCert.git'
              sh '''sudo docker build -t myapp1 .
                  sudo docker run -itd -p 8081:80 myapp1'''
            }
        }
        stage('Test the App'){
            agent {label 'test-slave'}
            steps{
                git changelog: false, poll: false, url: 'https://github.com/kambale-aishwarya/ED_SeleniumTest.git'
                sh 'mvn test'
            }
        }
    }
}
