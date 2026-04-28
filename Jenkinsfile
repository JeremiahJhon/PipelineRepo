pipeline {
    agent any

    stages {

        stage('Clone RepoA') {
            steps {
                git url: 'https://github.com/JeremiahJhon/RepoA.git'
            }
        }

        stage('Generate Doxygen Config') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'doxygen -g Doxyfile'
                    } else {
                        bat 'doxygen -g Doxyfile'
                    }
                }
            }
        }

        stage('Modify Config') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                        sed -i 's|^INPUT .*|INPUT = src|' Doxyfile
                        sed -i 's|^GENERATE_HTML .*|GENERATE_HTML = YES|' Doxyfile
                        sed -i 's|^GENERATE_LATEX .*|GENERATE_LATEX = NO|' Doxyfile
                        '''
                    } else {
                        powershell '''
                        (Get-Content Doxyfile) -replace '^INPUT .*', 'INPUT = src' |
                        Set-Content Doxyfile

                        (Get-Content Doxyfile) -replace '^GENERATE_HTML .*', 'GENERATE_HTML = YES' |
                        Set-Content Doxyfile

                        (Get-Content Doxyfile) -replace '^GENERATE_LATEX .*', 'GENERATE_LATEX = NO' |
                        Set-Content Doxyfile
                        '''
                    }
                }
            }
        }

        stage('Run Doxygen') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'doxygen Doxyfile'
                    } else {
                        bat 'doxygen Doxyfile'
                    }
                }
            }
        }

        stage('Archive Docs') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                        if [ -d html ]; then
                            tar -czf doc.tar.gz html
                        else
                            echo "HTML folder not found"
                            exit 1
                        fi
                        '''
                    } else {
                        powershell '''
                        if (Test-Path html) {
                            tar -czf doc.tar.gz html
                        } else {
                            Write-Error "HTML folder not found"
                            exit 1
                        }
                        '''
                    }

                    archiveArtifacts artifacts: 'doc.tar.gz', fingerprint: true
                }
            }
        }
    }

    post {
        success {
            echo "Build successful: documentation generated."
        }
        failure {
            echo "Build failed. Check logs."
        }
        always {
            cleanWs()
        }
    }
}