pipeline {
    agent any

    environment {
        DISCORD_WEBHOOK = credentials('DISCORD_WEBHOOK_URL')
    }

    stages {
        stage('Preparation') {
            steps {
                echo "🔧 Jenkins pipeline has started."
                script {
                    sendDiscordMessage("🛠️ Build started for `${env.JOB_NAME}` (#${env.BUILD_NUMBER})")
                }
            }
        }

        stage('Test Step') {
            steps {
                echo "Running some test commands..."
                sh 'echo Hello, this is a test.'
            }
        }
    }

    post {
        success {
            script {
                sendDiscordMessage("✅ Build **SUCCESS** for `${env.JOB_NAME}` (#${env.BUILD_NUMBER})")
            }
        }
        failure {
            script {
                sendDiscordMessage("❌ Build **FAILED** for `${env.JOB_NAME}` (#${env.BUILD_NUMBER})")
            }
        }
    }
}

// ✅ Reusable function for Discord messages
def sendDiscordMessage(String msg) {
    sh """
    curl -H "Content-Type: application/json" \
         -X POST \
         -d '{"content": "${msg}"}' \
         "${env.DISCORD_WEBHOOK}"
    """
}

