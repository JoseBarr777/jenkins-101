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
        APP_DIR  = "myapp"
    }

    stages {
        stage('Build') {
            steps {
                echo "Building.."
                sh '''
                set -euxo pipefail
                cd $APP_DIR

                echo "Creating/using virtualenv at $VENV_DIR"
                python3 --version

                # Create venv (fresh each build to avoid stale deps; remove rm -rf if you want reuse)
                rm -rf $VENV_DIR
                python3 -m venv $VENV_DIR

                # Use venv pip explicitly (avoids PEP 668 system pip error)
                ./$VENV_DIR/bin/python -m pip install --upgrade pip setuptools wheel
                ./$VENV_DIR/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Testing.."
                sh '''
                set -euxo pipefail
                cd $APP_DIR

                # Run tests with venv python
                ./$VENV_DIR/bin/python hello.py
                ./$VENV_DIR/bin/python hello.py --name=Brad
                '''
            }
        }

        stage('Deliver') {
            steps {
                echo 'Deliver....'
                sh '''
                set -euxo pipefail
                echo "doing delivery stuff.."
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
    }
}
