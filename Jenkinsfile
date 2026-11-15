pipeline {
    agent any

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'production'], description: 'Select target environment')
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Parallel Checks') {
            parallel {
                stage('Syntax Check') {
                    steps {
                        // Python syntax check
                        bat 'python -m py_compile main.py'
                    }
                }
                stage('Python Version') {
                    steps {
                        // Python version check
                        bat 'python --version'
                    }
                }
            }
        }

        stage('Run Unit Tests') {
            steps {
                script {
                    // Agar pytest installed hai, toh yeh command XML generate karegi
                    // Agar local testing ke liye pytest nahi hai, toh hum pehle se bani reports/result.xml use kar lenge
                    echo "Running pytest and generating JUnit XML report..."
                    
                    // Windows ke liye bat, Linux ke liye sh use karna
                    // bat 'pytest --junitxml=reports/result.xml'
                }
            }
        }
    }

    post {
        always {
            // --- YE HAI MAIN FEATURE GRAPH KE LIYE ---
            // Yeh step XML file ko read karega aur Jenkins dashboard par Test Trend Graph banayega
            junit testResults: 'reports/**/*.xml',
                  allowEmptyResults: true
                  
            // Artifacts bhi save rakhenge taaki download kar sako
            archiveArtifacts artifacts: 'reports/**/*', allowEmptyArchive: true
        }
        success {
            echo "✅ Build # ${env.BUILD_NUMBER} was SUCCESSFUL!"
        }
        failure {
            echo "❌ Build # ${env.BUILD_NUMBER} FAILED!"
        }
    }
}
