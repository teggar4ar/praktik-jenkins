pipeline {
    agent any

    stages {
        stage ('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage ('Run Tests') {
            steps {
                sh 'pytest test-app.py'
            }
        }

        stage ('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comparator: "REGEXP"
                }
            }
            steps {
                echo "Simulating deploy from branch: ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: "Build SUCCESS on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discordapp.com/api/webhooks/1369340438712156322/mCftVes_D3ARfdHVnB82rtZDdXE81XMAzAZZ9c9yqGBNmeo-w-TvCz5dAEO3oofS0bd8',
                )
            }
        }
        failure {
            script {
                def payload = [
                    content: "Build FAILURE on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discordapp.com/api/webhooks/1369340438712156322/mCftVes_D3ARfdHVnB82rtZDdXE81XMAzAZZ9c9yqGBNmeo-w-TvCz5dAEO3oofS0bd8',
                )
            }
        }
    }
}