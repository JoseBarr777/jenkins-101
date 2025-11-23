pipeline {
    agent { 
        node {
            label 'docker-agent-python'
        }
    }
    triggers {
        pollSCM('* * * * *')
    }
    environment {
        VENV_DIR = "venv"
    }
    stages {
        stage('Build') {
            steps {
                echo "Building.."
                sh '''
                set -e
                cd myapp

                # create venv if it doesn't exist
                python3 -m venv ${VENV_DIR}

                # activate venv
                . ${VENV_DIR}/bin/activate

                # upgrade pip tooling (optional but recommended)
                pip install --upgrade pip setuptools wheel

                # install deps into venv
                pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Testing.."
                sh '''
                set -e
                cd myapp
                . ${VENV_DIR}/bin/activate

                python3 hello.py
                python3 hello.py --name=Brad
                '''
            }
        }

        stage('Deliver') {
            steps {
                echo 'Deliver....'
                sh '''
                echo "doing delivery stuff.."
                '''
            }
        }
    }
}
