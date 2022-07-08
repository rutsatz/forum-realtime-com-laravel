// Defino uma variável que pode ser alterada
// https://www.youtube.com/watch?v=41uUsWQjKRw
def animal="cat"
pipeline {
    // Quero executar o passo do ansible no nó master, por isso eu defino cada nó separado.
    agent none

    stages {
        stage('Build') {
            // when {
            //     branch 'develop'
            // }
            // Digo em qual nó eu quero que o build seja executado.
            agent { node {label 'jenkins-worker'}}
            steps {
                sh 'composer update'
                sh 'composer install'
            }
        }
        // Configura o banco de dados caso ainda não estiver configurado.
        stage('Pre-Testing') {
            agent { node {label 'master'}}
            steps {
                sh 'ansible-playbook /home/easyit/laravel/mysql.yaml -i /home/easyit/laravel/mysql'
            }
        }
        stage('Test') {
            // Digo em qual nó eu quero que o build seja executado.
            agent { node {label 'jenkins-worker'}}
            steps {
                sh 'vendor/bin/phpunit'
            }
            post {
                failure {
                    emailext subject: "Job '${env.JOB_NAME}' '${env.BUILD_NUMBER }' ",
                        body: "<p>A build falhou at '${env.BUILD_URL}'</p>",
                        recipientProviders: [developers(), requestor()]
                }
                success {
                    emailext subject: "Job '${env.JOB_NAME}' '${env.BUILD_NUMBER }' ",
                        body: "<p>A build foi feita com sucesso '${env.BUILD_URL}'</p>",
                        to: 'rafa.rutsatz@gmail.com'
                }
            }
        }
    }
}