pipeline{

    agent any

    tools {

        nodejs 'Node20'
    }

    environment{

        DOCKER_REGISTRY = "shady25/zoomclone"
        DOCKER_CRED = "DOCKER_CREDS"
        GITHUB_REPO_NAME = "Zoom-Clone-K8s"
        GITHUB_REPO_URL = "https://github.com/shadyosama9/Zoom-Clone-K8s.git"
        GITHUB_USERNAME = "shadyosama9"
        GITHUB_EMAIL = "shadyosama554@gmail.com"
        ZOOM_K8s_IMAGE = "shady25/zoomclone"
    }

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
                withSonarQubeEnv('SonarCloud'){
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=zoom-clone-project \
                        -Dsonar.projectName=Zoom-Clone-Project \
                        -Dsonar.organization=zoom-clone \
                        -Dsonar.projectVersion=1.0 
                        '''
                }
            }
        }


        stage('Install Dependencies For Check'){
            steps{
                sh 'npm install'
            }
        }

        stage('Running Dependecncy Check'){
            steps{
                dependencyCheck additionalArguments: '-f XML --disableYarnAudit -s .',
                          odcInstallation: 'Zoom-OWASP'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }

        stage('Running Trivy Analysis'){
            steps{
                sh 'trivy fs . > trivy-code.txt'
            }
        }

        stage('Building Docker Image'){
            steps{
                script{
                    dockerImg = docker.build DOCKER_REGISTRY + ":V$BUILD_NUMBER"
                }
            }
        }

        stage('Running Trivy On Docker'){
            steps{
                sh "trivy image $DOCKER_REGISTRY:V$BUILD_NUMBER  > trivy-docker.txt"
            }
        }

        stage('Uploading To DokcerHub'){
            steps{
                script {
                    docker.withRegistry('', DOCKER_CRED){
                        dockerImg.push ("V$BUILD_NUMBER")
                    }
                }
            }        
        }

        stage('CleanUp'){
            steps{

                sh "docker system prune -f"
                sh "rm -rf node_modules"
            }
        }



        stage('Pushing Image Version To K8s Repo'){
            steps{
                    withCredentials([string(credentialsId: 'GitHub-Token', variable: 'GITHUB_TOKEN')]) {
                        sh '''

                            mkdir -p k8s-temp
                            cd k8s-temp
                            
                            git clone ${GITHUB_REPO_URL}

                            cd Zoom-Clone-K8s
                            sed -i "s#${ZOOM_K8s_IMAGE}:V[0-9]*#${ZOOM_K8s_IMAGE}:V$BUILD_NUMBER#g" ./kubernetes/zoom-deploy.yml
                            
                            git config --global user.email "${GITHUB_EMAIL}"
                            git config --global user.name "${GITHUB_USERNAME}"

                            git add .
                            git commit -m "changing image tag to V:$BUILD_NUMBER"
                            git push https://${GITHUB_TOKEN}@github.com/${GITHUB_USERNAME}/${GITHUB_REPO_NAME} HEAD:main


                        '''
                        sh "rm -rf k8s-temp/"
                }
            }
        }
    }
}