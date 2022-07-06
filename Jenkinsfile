// Defino uma variável que pode ser alterada
// https://www.youtube.com/watch?v=41uUsWQjKRw
def animal="cat"
pipeline {
    // Digo em qual nó eu quero que o build seja executado.
    agent { node {label 'jenkins-worker'}}
    stages {
        stage('Build') {
            // when {
            //     branch 'develop'
            // }
            steps {
                sh 'composer update'
                sh 'composer install'
            }
        }
        stage('Test') {
            steps {
                sh 'vendor/bin/phpunit'
            }
            post {
                failure {
                    emailext subject: "Job '${env.JOB_NAME}' '${env.BUILD_NUMBER }' ",
                        body: "<p>Check console output at '${env.BUILD_URL}'</p>",
                        recipientProviders: [developers(), requestor()]
                }
                success {
                    emailext subject: "Job '${env.JOB_NAME}' '${env.BUILD_NUMBER }' ",
                        body: "<p>Check console output at '${env.BUILD_URL}'</p>",
                        to: 'rafa.rutsatz@gmail.com'
                }
            }
        }
    }
}