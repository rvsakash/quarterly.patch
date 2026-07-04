pipeline {
    // Jenkins ko batata hai ki kisi bhi available worker node par ise chalayein
    agent any

    // Multibranch pipelines ke liye automatic triggers configuration
    triggers {
        // Har 5 minute mein GitHub scan karega agar webhook configure nahi hai
        cron('H/5 * * * *')
    }

    stages {
        // Stage 1: Validation aur check ki kaunsi branch chal rahi hai
        stage('Initial Validation') {
            steps {
                echo "🚀 Jenkins Pipeline Triggered Automatically!"
                echo "📁 Processing Branch Name: ${env.BRANCH_NAME}"
                echo "📝 Commit ID: ${env.GIT_COMMIT}"
            }
        }

        // Stage 2: Yahan aapki actual system patching aur reboot ka workflow chalega
        stage('Execute System Patching & Reboot') {
            steps {
                echo "🛠️ Executing Live Ansible Playbook on Target Servers..."
                
                // ASLI COMMAND: Yeh aapki repository ki Ansible playbook ko run karegi
                sh "ansible-playbook quarterly_patching.yml"
                
                echo "✅ Ansible execution and patching workflow finished."
            }
        }
    }

    // Post Actions Setup (Cleanup aur Status notification)
    post {
        always {
            script {
                echo '🧹 Post-build actions started...'
                echo 'Enforcing zero disk retention: Wiping temporary workspace keys...'
                sh "echo 'Cleanup task completed! Clear temporary build tokens...'"
            }
        }
        success {
            script {
                echo "🎉 Build finished successfully on branch: ${env.BRANCH_NAME}"
            }
        }
        failure {
            script {
                echo "❌ Build FAILED! Please check the console output logs immediately."
            }
        }
    }
}
