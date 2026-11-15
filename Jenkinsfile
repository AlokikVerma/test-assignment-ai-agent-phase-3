pipeline {
    agent any

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'production'], description: 'Select target environment')
    }

    stages {
        stage('Checkout Code') {
            steps {
                // GitHub se latest code pull karega
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                script {
                    echo "🚀 Running build for environment: ${params.ENVIRONMENT}"
                    
                    // Windows machine par hai toh bat/cmd use hoga, Linux par sh
                    // Yahan apne test cases ya build commands daal sakta hai
                    if (isUnix()) {
                        sh 'echo Building application...'
                    } else {
                        bat 'echo Building application on Windows...'
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build # ${env.BUILD_NUMBER} was SUCCESSFUL!"
            
            // 1. Reports ya Artifacts Archive karna (Jo tune bola)
            // Iske liye pehle project me kahin reports/ folder me files honi chahiye, 
            // allowEmptyArchive: true rakha hai taaki agar folder na ho toh build fail na ho.
            archiveArtifacts artifacts: 'reports/**/*', allowEmptyArchive: true

            script {
                // 2. Success Notification Feature (Console log alert / Custom webhook message)
                // Yahan tu Slack ya Discord ka webhook URL bhi daal sakta hai sh 'curl ...' karke
                echo "🎉 SUCCESS NOTIFICATION: Job '${env.JOB_NAME}' [Build #${env.BUILD_NUMBER}] completed successfully on ${params.ENVIRONMENT} environment!"
            }
        }
        failure {
            echo "❌ Build # ${env.BUILD_NUMBER} FAILED! Check console output for errors."
        }
    }
}
