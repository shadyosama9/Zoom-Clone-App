pipeline{

    agent any

    stages {

        stage('Create Or Update .env File') {
            steps {
                script {
                    if (!fileExists('.env')) {
                        withCredentials([file(credentialsId: 'ENV', variable: 'SECRET_FILE')]) {
                            sh "touch .env"
                            sh "cat \"$SECRET_FILE\" > .env"
                        }
                    } else {
                        withCredentials([file(credentialsId: 'ENV', variable: 'NEW_FILE')]) {
                            def secretContent = readFile(env.NEW_FILE).trim()
                            def envContent = readFile('.env').trim()
                            if (secretContent != envContent) {
                                writeFile file: '.env', text: secretContent, encoding: 'UTF-8'
                                echo '.env file updated'
                            } else {
                                echo '.env file is up to date'
                            }
                        }
                    }
                }
            }
        }



        stage('Sonar Cloud Analysis'){
            environment{
                scannerHome = tool 'Zoom-Sonar'
            }

            steps {

                sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=zoom-clone-project \
                    -Dsonar.projectName=Zoom-Clone-Project \
                    -Dsonar.organization=zoom-clone \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.host.url=https://sonarcloud.io
                    '''
            }
        }
    }

}