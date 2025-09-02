pipeline {
    agent { label 'AGENT-1' }

    environment {
        person = "saiteja"
    }

    stages {
        stage('Build') {
            steps {
                script {

                    sh """
                        echo "Hello Build"
                        sleep 10
                        echo " Hi ${person}"
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