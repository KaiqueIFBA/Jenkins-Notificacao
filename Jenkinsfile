pipeline {
    agent any
    stages {
        stage('Check Internet Speed') {
            steps {
                script {
                    
                    pip install speedtest-cli

                    def speedTestResult = sh(script: "speedtest-cli --json", returnStdout: true).trim()

                    def speedTestJson = new groovy.json.JsonSlurperClassic().parseText(speedTestResult)

                    def downloadSpeedMbps = speedTestJson.download / 1000000 // Convert to Mbps

                    echo "Download Speed: ${downloadSpeedMbps} Mbps"

                    if (downloadSpeedMbps < 10) {
                        currentBuild.result = 'FAILURE'
                        error "Internet speed is below 10 Mbps"
                    }
                }
            }
        }
    }
    post {
        always {
            emailext (
                subject: "Notificação de Velocidade de Internet - ${currentBuild.fullDisplayName}",
                body: "A velocidade da internet está abaixo de 10 Mbps.",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                attachLog: true,
                compressLog: true,
                replyTo: "bekahmarques17@gmail.com"
            )
        }
    }
}
