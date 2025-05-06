pipeline {
    agent any

    environment {
        BRANCH_NAME = 'main'  // Set variabel BRANCH_NAME jika tidak ada
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
                expression { env.BRANCH_NAME == 'main' }
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
                expression { env.BRANCH_NAME == 'main' }
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
            discordSend (webhookURL: 'https://discord.com/api/webhooks/1369162621785477182/wI5eMHiepamlIck7OrDXDRfRgSygbwQCZYr0VwMJUQY5O74AYUg-8ZtiOHFsnKaKQWMr', message: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
        }
        failure {
            discordSend (webhookURL: 'https://discord.com/api/webhooks/1369162621785477182/wI5eMHiepamlIck7OrDXDRfRgSygbwQCZYr0VwMJUQY5O74AYUg-8ZtiOHFsnKaKQWMr', message: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
        }
    }
}
