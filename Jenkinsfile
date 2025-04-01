pipeline {
    agent any
    environment {
        LABS = credentials('labcreds')
        PIPENV_HOME = "${WORKSPACE}/.local/bin"
        PATH = "${PIPENV_HOME}:${PATH}"
    }
    stages {
        stage('Check Dependencies') {
            steps {
                sh '''
                    echo "Checking Python and Pip versions..."
                    if ! command -v python3 &> /dev/null; then
                        echo "Python3 not found! Install manually if required."
                        exit 1
                    fi
                    if ! command -v pip3 &> /dev/null; then
                        echo "Installing Pip3..."
                        curl -sS https://bootstrap.pypa.io/get-pip.py | python3 --user
                        export PATH="${HOME}/.local/bin:${PATH}"
                    fi
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                    export PATH="${HOME}/.local/bin:${PATH}"
                    pip3 install --user pipenv
                    pipenv --rm || exit 0
                    pipenv install
                '''
            }
        }
        stage('Test') {
            steps {
                sh 'pipenv run pytest'
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
