
pipeline {
    agent any

    stages {
        stage('Check Python') {
            steps {
                bat 'python --version'
            }
        }

        stage('Syntax Check') {
            steps {
                bat 'python -m py_compile main.py'
            }
        }

        stage('Run Application') {
            steps {
                bat 'python main.py'
            }
        }
    }
}
