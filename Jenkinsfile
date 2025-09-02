pipeline {
    agent { label 'AGENT-1' }

    environment {
       Course = "Devops with Jenkins"
    }

    stages {
        stage('Build') {
            steps {
                script {

                    sh """
                        echo "Hello Build"
                        sleep 10
                        env
                    """
                }
               

            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}