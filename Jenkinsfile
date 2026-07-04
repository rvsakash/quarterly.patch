pipeline {
    agent any

    stages {
        stage('Example Stage') {
            steps {
                echo 'Running patching workflows...'
            }
        }
    }

    post {
        always {
            // AGAR SH RUN KARNA HAI, TOH USE HAMESHA NODE KE ANDAR RAKHEIN
            node {
                echo 'Enforcing zero disk retention: Wiping temporary workspace keys...'
                // Aapka sh command yahan safe rahega
                sh "echo 'Cleaning up...'" 
            }
        }
    }
}
