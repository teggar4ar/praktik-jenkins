pipeline {
    agent {
        docker {
            image 'python:3.10'
        }
    }

    environment {
        VENV = 'venv'
    }

    stages {
        stage ('Setup Environment & Install Dependencies') {
            steps {
                sh '''
                    python -m venv $VENV
                    . $VENV/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage ('Run Tests') {
            steps {
                sh '''
                    . $VENV/bin/activate
                    pytest test-app.py
                '''
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
