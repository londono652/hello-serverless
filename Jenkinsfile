pipeline{
    agent any
    tools {nodejs "nodejs"}
    stages{
        stage('build Sin Test') {
            steps{
                sh 'npm install'
                sh 'npm rebuild'
                sh 'npm run build --skip-test --if-present'
            }
        }
        stage('unitTest') {
            steps {
                nodejs(nodeJSInstallationName: 'nodejs') {
                    sh 'npm run test:coverage && cp coverage/lcov.info lcov.info || echo "Code coverage failed" '
                archiveArtifacts(artifacts: 'coverage/**', onlyIfSuccessful: true)
                }
            }
        }
        stage('analisis estatico') {
            steps{
                sh 'echo analisis estatico'
            }
        }
        stage('deploy') {
            steps{
                nodejs(nodeJSInstallationName: 'nodejs') {
                    withAWS(credentials: 'aws-credentials') {
                        sh 'serverless deploy'
                    }
                }
            }
        }
    }
}