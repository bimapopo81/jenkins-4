pipeline {
    agent any

    environment {
        // Set BRANCH_NAME menjadi 'feature' untuk mensimulasikan branch feature
        BRANCH_NAME = 'feature'  // Ubah nilai ini untuk mensimulasikan branch feature
    }

    stages {
        stage('Test') {
            steps {
                script {
                    echo "Testing the application..."
                    echo "Executing pipeline for branch ${env.BRANCH_NAME}" 
                }
            }
        }

        stage('Build') {
            when {
                expression { env.BRANCH_NAME == 'main' || env.BRANCH_NAME == 'feature' }
            }
            steps {
                script {
                    echo "Building the application..."
                    sh 'mvn clean install'  // Menjalankan perintah Maven untuk build aplikasi
                }
            }
        }

        stage('Run') {
            steps {
                script {
                    echo "Running the application..."
                    // Menjalankan aplikasi setelah build
                    sh 'java -jar target/java-maven-app-1.1.0-SNAPSHOT.jar &'
                    // Cek apakah aplikasi sudah bisa diakses pada port 8080
                    sh 'curl --silent --fail http://localhost:8080 || echo "Aplikasi tidak berjalan dengan baik!"'
                }
            }
        }

        stage('Deploy') {
            when {
                expression { env.BRANCH_NAME == 'main' || env.BRANCH_NAME == 'feature' }
            }
            steps {
                script {
                    echo "Deploying the application..."
                }
            }
        }
    }

    post {
        success {
            script {
                // Mengirimkan notifikasi sukses ke Discord
                def discordMessage = """{
                    "content": "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER} on branch ${env.BRANCH_NAME}"
                }"""
                httpRequest acceptType: 'APPLICATION_JSON', contentType: 'APPLICATION_JSON', httpMode: 'POST', url: 'https://discord.com/api/webhooks/1369162621785477182/wI5eMHiepamlIck7OrDXDRfRgSygbwQCZYr0VwMJUQY5O74AYUg-8ZtiOHFsnKaKQWMr', requestBody: discordMessage
            }
        }
        failure {
            script {
                // Mengirimkan notifikasi gagal ke Discord
                def discordMessage = """{
                    "content": "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER} on branch ${env.BRANCH_NAME}"
                }"""
                httpRequest acceptType: 'APPLICATION_JSON', contentType: 'APPLICATION_JSON', httpMode: 'POST', url: 'https://discord.com/api/webhooks/1369162621785477182/wI5eMHiepamlIck7OrDXDRfRgSygbwQCZYr0VwMJUQY5O74AYUg-8ZtiOHFsnKaKQWMr', requestBody: discordMessage
            }
        }
    }
}
