pipeline {
	agent any
	environment {
		LABS = credentials('labcreds')
		}
	stages {
		stage('Build') {
			steps {
				sh 'pip3 install --user pipenv'
				sh '/bitnami/jenkins/home/.local/bin/pipenv --rm || exit 0'
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
				sh 'sshpass -p $LABS_PSW scp -o StrictHostKeyChecking=no -r .
				$LABS_USR@g02.itversity.com:/home/itv015605/retailproject'
				}
			}
		}
	}