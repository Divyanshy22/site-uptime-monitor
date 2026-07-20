pipeline {
    agent any

    triggers {
        cron('H/5 * * * *')
    }

    environment {
        PATH = "/opt/homebrew/bin:/usr/local/bin:${env.PATH}"
        SITE_URL = 'http://divyansh-jenkins-static-site-2026.s3-website-us-east-1.amazonaws.com'
    }

    stages {
        stage('Check Site Health') {
            steps {
                script {
                    def statusCode = sh(
                        script: "curl -s -o /dev/null -w '%{http_code}' ${env.SITE_URL}",
                        returnStdout: true
                    ).trim()

                    echo "Checked ${env.SITE_URL} — HTTP status: ${statusCode}"

                    if (statusCode != '200') {
                        error("Site health check failed! Status code: ${statusCode}")
                    } else {
                        echo "Site is healthy."
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Health check passed at ${new Date()}"
        }
        failure {
            echo "ALERT: Site appears to be down!"
        }
    }
}
