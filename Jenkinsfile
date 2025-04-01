pipeline {
    agent any

    stages {
        stage('Setup Environment') {
            steps {
                // Check if Python3 and pip3 are available
                sh 'which python3 || echo "Python3 not found"'
                sh 'which pip3 || echo "pip3 not found"'

                // Install Python3 and pip3 if they are missing (for Debian/Ubuntu)
                sh '''
                if ! command -v python3 &> /dev/null; then
                    echo "Installing Python3..."
                    apt-get update && apt-get install -y python3 python3-pip
                fi
                if ! command -v pip3 &> /dev/null; then
                    echo "Installing pip3..."
                    apt-get install -y python3-pip
                fi
                '''
            }
        }

        stage('Build') {
            steps {
                // Upgrade pip and install pipenv using the correct module "pip"
                sh '/usr/bin/python3 -m pip install --upgrade pip'
                sh '/usr/bin/python3 -m pip install pipenv'

                // Ensure pipenv is in the PATH (adjust if necessary)
                sh 'export PATH=$HOME/.local/bin:$PATH'

                // Remove existing Pipenv environment gracefully
                sh '/bitnami/jenkins/home/.local/bin/pipenv --rm || exit 0'

                // Install dependencies using Pipenv
                sh '/bitnami/jenkins/home/.local/bin/pipenv install'
            }
        }
		stage('Test') {
			steps {
				sh '/bitnami/jenkins/home/.local/bin/pipenv run pytest'
			}
		}
		stage('Package') {
			steps {
				sh 'zip -r retailproject.zip .'
			}
		}
		stage('Deploy') {
			steps {
				sh 'sshpass -p $LABS_PSW scp -o StrictHostKeyChecking=no -r . $LABS_USR@g02.itversity.com:/home/itv015605/retailproject'
			}
		}
	}
}