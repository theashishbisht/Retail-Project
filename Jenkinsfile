pipeline {
    agent any
    environment {
        LABS = credentials('labcreds')
        PATH = "/usr/local/bin:/usr/bin:/bin:$HOME/.local/bin:$PATH"
    }
    stages {
        stage('Build') {
            steps {
                sh '/usr/bin/pip3 install --user pipenv'
                sh '$HOME/.local/bin/pipenv --rm || exit 0'
                sh '$HOME/.local/bin/pipenv install'
            }
        }
        stage('Test') {
            steps {
                sh '$HOME/.local/bin/pipenv run pytest'
            }
        }
        stage('Package') {
            steps {
                sh 'zip -r retailproject.zip .'
            }
        }
        stage('Deploy') {
            steps {
                sh 'sshpass -p "$LABS_PSW" scp -o StrictHostKeyChecking=no -r . "$LABS_USR@g02.itversity.com:/home/itv015605/retailproject"'
            }
        }
    }
}
