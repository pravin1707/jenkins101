pipeline {
    agent { 
        node {
            label 'agent-python-docker'
            }
      }
      triggers {
          pollSCM('* * * * *')
      }
    stages {
        stage('Build') {
            steps {
                echo "Building.."
                sh '''
                cd myapp
                
                if [ ! -d "$VENV_PATH" ]; then
                    echo "Creating virtual environment..."
                    python3 -m venv $VENV_PATH
                fi
                
                . $VENV_PATH/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }
        stage('Test') {
            steps {
                echo "Testing.."
                sh '''
                cd myapp

                . $VENV_PATH/bin/activate
                
                python3 hello.py
                python3 hellp.py --name=Pravin
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
