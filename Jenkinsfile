pipeline {
    agent any
    environment {
        LABS = credentials('labcreds')
    }
    stages {
        stage('Build') {
            steps {
                sh 'pip3 install --user pipenv'
                sh ''' 
                    PIPENV_PATH=$(which pipenv)
                    $PIPENV_PATH --rm || exit 0
                    $PIPENV_PATH install
                '''
            }
        }
        stage('Test') {
            steps {
                sh '$(which pipenv) run pytest'
            }
        }
        stage('Package') {
            steps {
                sh 'zip -r retailproject.zip .'
            }
        }
        stage('Deploy') {
            steps {
                sh ''' 
                    sshpass -p "$LABS_PSW" scp -o StrictHostKeyChecking=no -r . "$LABS_USR@g02.itversity.com:/home/itv015605/retailproject"
                '''
            }
        }
    }
}