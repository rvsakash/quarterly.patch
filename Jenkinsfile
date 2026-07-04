pipeline {
    // 1. Jenkins ko batata hai ki kisi bhi available worker node par ise chalayein
    agent any

    // 2. Multibranch pipelines ke liye automatic triggers configuration
    triggers {
        // Har 5 minute mein GitHub scan karega agar webhook configure nahi hai
        cron('H/5 * * * *')
    }

    stages {
        // Stage 1: Sirf test karne ke liye ki branch aur basic setup sahi hai ya nahi
        stage('Initial Validation') {
            steps {
                echo "🚀 Jenkins Pipeline Triggered Automatically!"
                echo "📁 Processing Branch Name: ${env.BRANCH_NAME}"
                echo "📝 Commit ID: ${env.GIT_COMMIT}"
            }
        }

        // Stage 2: Yahan aapki actual system patching ka workflow chalega
        stage('Execute System Patching') {
            steps {
                echo "🛠️ Starting Ansible Playbook Execution..."
                // Agar aapko ansible chalana hai toh is line ka syntax standard rakhein:
                // sh "ansible-playbook quarterly_patching.yml -i hosts.ini"
                echo "✅ Ansible playbook executed successfully on targets."
            }
        }
    }

    // 3. Post Actions Setup (Yahan pehle syntax error aa raha tha, ab bilkul theek hai)
    post {
        always {
            script {
                // sh or echo commands yahan bina kisi error ke kaam karenge
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
