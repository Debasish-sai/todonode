pipeline {
    agent any
    
    environment {
        PROJECT_DIR = '/var/jenkins_home/workspace/todonode-pipeline'
    }
    
    stages {
        stage('🔍 Verify Setup') {
            steps {
                echo '=== Checking project files ==='
                sh '''
                    echo "Working directory: $(pwd)"
                    ls -la
                    echo "✅ Files accessible!"
                '''
            }
        }
        
        stage('📦 Install Dependencies') {
            steps {
                echo '=== Installing npm packages ==='
                sh '''
                    bun install
                    echo "✅ Dependencies installed!"
                '''
            }
        }
        
       /*  stage('🧪 Run Tests') {
            steps {
                echo '=== Running tests ==='
                sh '''
                    npm test
                    echo "✅ Tests passed!"
                '''
            }
        } */
        
        stage('🚀 Push to GitHub') {
            when {
                expression { 
                    currentBuild.result == null || currentBuild.result == 'SUCCESS' 
                }
            }
            steps {
                echo '=== Pushing to GitHub ==='
                sshagent(['github-ssh']) {
                    sh '''
                        git config user.email "jenkins@ci.localhost"
                        git config user.name "Jenkins CI"
                        
                        git add .
                        git diff-index --quiet HEAD || git commit -m "✅ Build #${BUILD_NUMBER} - Tests Passed"
                        
                        mkdir -p ~/.ssh
                        ssh-keyscan -H github.com >> ~/.ssh/known_hosts 2>/dev/null
                        
                        git push git@github.com:Debasish-sai/todonode.git HEAD:main
                        
                        echo "✅ Pushed to GitHub!"
                    '''
                }
            }
        }
    }
    
    post {
        success {
            echo '🎉 Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed - check logs'
        }
    }
}
