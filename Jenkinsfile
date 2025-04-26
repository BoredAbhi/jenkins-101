pipeline {
    agent { 
        node {
            label 'docker-agent-python'
            }
      }
    parameters {
        choice(name: 'ENV', choices: ['dev', 'qa', 'prod'], description: 'Choose environment to run against')
    }
    stages {
        stage('Build') {
            steps {
                echo "Building...."
                sh '''
                cd myapp
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                deactivate
                '''
            }
        }
        stage('Test') {
            steps {
                echo "Testing.."
                withCredentials([usernamePassword(credentialsId: 'my-creds-id', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh '''
                echo "Using username $USERNAME"
                echo "Using password $PASSWORD"
                cd myapp
                . venv/bin/activate
                python3 hello.py
                python3 hello.py --name=abhi
                deactivate
                '''
            }
        }
        }
        stage('Deploy') {
            steps {
                echo "Deploying to ${params.ENV} environment"
                sh """
                echo "doing delivery stuff... Deploying to ${params.ENV} environment"
                """
            }
        }
    }
    post {
        always {
            echo 'This always runs, cleanup or notifications.'
        }
        success {
            echo 'Build succeeded! Send success notification.'
        }
        failure {
            echo 'Build failed! Send failure alert.'
        }
        unstable {
            echo 'Build is unstable! (maybe tests failed)'
        }
        changed {
            echo 'Build status changed from last run.'
        }
    }

}
