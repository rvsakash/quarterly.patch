pipeline {
    agent any

    triggers {
        // Yeh har 5 minute mein GitHub scan karega aur naye code par automatic chalega
        cron('H/5 * * * *')
    }

    environment {
        // Yeh line Jenkins se password utha kar ek variable mein save karegi
        VAULT_PASS = credentials('ansible-vault-pass')
    }

    stages {
        stage('Initial Validation') {
            steps {
                echo "🚀 Jenkins Pipeline Triggered Automatically!"
                echo "📁 Processing Branch Name: ${env.BRANCH_NAME}"
            }
        }

        stage('Execute System Patching & Reboot') {
            steps {
                echo "🛠️ Executing Live Ansible Playbook on Target Servers..."
                
                script {
                    // 1. Ek temporary file mein password likhein taaki Ansible use padh sake
                    sh 'echo "$VAULT_PASS" > .vault_pass.txt'
                    
                    try {
                        // 2. ASLI COMMAND: Yeh hosts.ini aur vault file dono ko lekar playbook chalayegi
                        sh "ansible-playbook quarterly_patching.yml -i hosts.ini --vault-password-file .vault_pass.txt"
                    } finally {
                        // 3. Security ke liye password file ko workspace se turant delete karein
                        sh 'rm -f .vault_pass.txt'
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                echo '🧹 Post-build actions completed. System clean!'
            }
        }
        success {
            script { echo "🎉 Patching and Reboot successful on branch: ${env.BRANCH_NAME}" }
        }
        failure {
            script { echo "❌ Build FAILED! Please check the console output logs." }
        }
    }
}
