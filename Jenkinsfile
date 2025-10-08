pipeline {
    agent any

    stages {
        // Stage ini akan selalu berjalan untuk semua branch
        stage('Test') {
            steps {
                script {
                    echo "Testing the application for branch ${env.BRANCH_NAME}"
                    // Untuk simulasi error, Anda bisa tambahkan 'error("Simulating a build failure")' di bawah ini
                }
            }
        }

        // Stage ini HANYA berjalan jika branch adalah 'main'
        stage('Build') {
            when {
                branch 'main'
            }
            steps {
                echo "Building the application..."
                // Contoh perintah build: sh 'mvn clean package'
            }
        }

        // Stage ini HANYA berjalan jika branch adalah 'main'
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo "Deploying the application..."
            }
        }
    }

    // Blok ini akan berjalan setelah semua stage selesai
    post {
        always {
            script {
                // Membersihkan workspace setelah build selesai
                cleanWs()
            }
        }
        success {
            // Mengirim notifikasi jika build SUKSES
            discordSend(
                webhookURL: 'YOUR_DISCORD_WEBHOOK_URL',
                message: "✅ Build SUKSES: Job `${env.JOB_NAME}` build `${env.BUILD_NUMBER}` berhasil.",
                title: "Build Succeeded",
                color: '#00ff00'
            )
        }
        failure {
            // Mengirim notifikasi jika build GAGAL
            discordSend(
                webhookURL: 'YOUR_DISCORD_WEBHOOK_URL',
                message: "❌ Build GAGAL: Job `${env.JOB_NAME}` build `${env.BUILD_NUMBER}` gagal. Cek log di ${env.BUILD_URL}",
                title: "Build Failed",
                color: '#ff0000'
            )
        }
    }
}
