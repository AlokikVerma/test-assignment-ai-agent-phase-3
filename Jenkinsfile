pipeline {
    agent any
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'production'], description: 'Select environment')
    }
    stages {
        stage('Deploy') {
            steps {
                script {
                    // Yahan check hota hai ki tune kya parameter diya hai
                    if (params.ENVIRONMENT == 'production') {
                        echo "⚠️ Deploying to LIVE Production Server (Safe mode)!"
                        sh 'bash deploy-prod.sh'
                    } else if (params.ENVIRONMENT == 'staging') {
                        echo "🧪 Deploying to Staging/Testing Server."
                        sh 'bash deploy-staging.sh'
                    } else {
                        echo "💻 Running on Local/Dev environment."
                        sh 'bash deploy-dev.sh'
                    }
                }
            }
        }
    }
}
