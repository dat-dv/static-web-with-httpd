pipeline {
    agent any
    triggers {
        githubPush()
    }
    environment {
        githubRepo = 'https://github.com/dat-dv/static-web-with-httpd.git'
        buildBranchName = 'Master'
        githubCredentialsId = 'github-creds'
        sshCredentialsId = 'id_rsa_jenkins'
        serverBuildFolder = '/home/dat-doan/projects/jenkins/out/static-web-with-httpd'
        serverUser = 'dat-doan'
        serverDomain = 'datserver.duckdns.org'
    }

    stages {
        stage('Build & Deploy') {
            when {
                anyOf {
                    buildingTag()
                    expression { env.BRANCH_NAME == null || env.BRANCH_NAME == env.buildBranchName }
                }
            }
            stages {  // ✅ Nested stages
                stage('Checkout') {
                    steps {
                        git branch: env.buildBranchName,
                            url: env.githubRepo,
                            credentialsId: env.githubCredentialsId
                    }
                }

                stage('Deploy') {
                    steps {
                        sshagent([env.sshCredentialsId]) {
                            sh """
                                echo "🚀 Copy code từ workspace sang host..."
                                rsync -avz --delete -e "ssh -o StrictHostKeyChecking=no" \
                                    ./ ${env.serverUser}@${env.serverDomain}:${env.serverBuildFolder}

                                echo "🔧 Deploy container trên host..."
                                ssh -o StrictHostKeyChecking=no ${env.serverUser}@${env.serverDomain} '
                                    cd ${env.serverBuildFolder}
                                    docker compose down || true
                                    docker rm -f static-httpd || true
                                    docker compose up -d --build
                                    sleep 2
                                    docker compose ps
                                    echo "✅ HTTPD is running"
                                '
                            """
                        }
                    }
                }

                stage('Clean Workspace') {
                    steps {
                        deleteDir()
                    }
                }
            }
        }
    }
}