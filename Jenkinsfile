pipeline {
    agent any

    // 1. Production Parameters: Isse Jenkins UI par dynamic input box ban jayega
    parameters {
        string(
            name: 'TARGET_IP', 
            defaultValue: '10.220.0.13', 
            description: 'Jis Client Node ko patch aur reboot karna hai, uska IP address yahan dalein.'
        )
    }

    triggers {
        cron('H/5 * * * *')
    }

    environment {
        VAULT_PASS = credentials('ansible-vault-pass')
    }

    stages {
        stage('Initial Validation') {
            steps {
                echo "🚀 Jenkins Pipeline Triggered!"
                echo "📁 Branch Name: ${env.BRANCH_NAME}"
                echo "🎯 Target Client IP to Patch: ${params.TARGET_IP}"
            }
        }

        stage('Execute System Patching & Reboot') {
            steps {
                echo "🛠️ Executing Live Ansible Playbook on Target Server: ${params.TARGET_IP}"
                
                script {
                    // 2. Secret Vault password file create karein
                    sh 'echo "$VAULT_PASS" > .vault_pass.txt'
                    
                    try {
                        // 3. Dynamic Injection: -e se target_ip ka variable pipeline se direct Ansible hosts.ini mein chala jayega
                        sh "ansible-playbook quarterly_patching.yml -i hosts.ini --vault-password-file .vault_pass.txt -e 'target_ip=${params.TARGET_IP}'"
                    } finally {
                        // 4. Safe Cleanup
                        sh 'rm -f .vault_pass.txt'
                    }
                }
            }
        }
    }

    post {
        always {
            script { 
                echo '🧹 Post-build actions completed. Environment is clean!' 
            }
        }
        success {
            script { 
                echo "🎉 SUCCESS: Patching and Reboot successfully finished on ${params.TARGET_IP}!" 
            }
        }
        failure {
            script { 
                echo "❌ FAILURE: Build failed for ${params.TARGET_IP}. Check logs above." 
            }
        }
    }
}
