pipeline {
    agent any
    environment {
        LABS = credentials('labcreds')
    }
    stages {
        stage('Check Dependencies') {
            steps {
                sh '''
                    echo "Checking Python and Pip versions..."
                    python3 --version || echo "Python3 not found"
                    pip3 --version || echo "Pip3 not found"
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                    if ! command -v pip3 &> /dev/null; then
                        echo "Installing Python3 and Pip3..."
                        sudo apt-get update && sudo apt-get install -y python3 python3-pip
                    fi
                    pip3 install --user pipenv
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
