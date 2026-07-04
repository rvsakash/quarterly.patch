pipeline {
    agent any
    
    environment {
        // Enforces secure password variable assignment from Jenkins Secrets Store
        VAULT_SECRET = credentials('ansible-vault-pass')
        TARGET_NODE_IP = '10.220.0.13'
    }
    
    stages {
        stage('Pre-Flight Repository Validation') {
            steps {
                echo 'Verifying system lifecycle workspace parameters...'
                sh 'ls -la'
            }
        }
        
        stage('Dynamic Secret Initialization') {
            steps {
                // Dynamically creates the hidden decryption token in execution runtime workspace
                sh 'echo -n "${VAULT_SECRET}" > .vault_pass.txt'
                sh 'chmod 600 .vault_pass.txt'
            }
        }
        
        stage('Execute Automated Patch Lifecycle') {
            steps {
                // Parallel orchestration executing configuration check with forks optimization
                sh "ansible-playbook -i hosts.ini quarterly_patching.yml --vault-password-file .vault_pass.txt -e 'target_ip=${TARGET_NODE_IP}' -f 5 | tee patching_execution_report.txt"
            }
        }
    }
    
    post {
        always {
            // Strict Security Control: Immediately purge cleartext encryption key from memory storage
            echo 'Enforcing zero disk retention: Wiping temporary workspace keys...'
            sh 'rm -f .vault_pass.txt'
            
            // Archives automation performance metrics output artifact direct to Jenkins UI dashboard
            archiveArtifacts artifacts: 'patching_execution_report.txt', allowEmptyArchive: true
        }
    }
}

