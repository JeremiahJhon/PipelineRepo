pipeline {
    agent any

    stages {
        stage('Clone RepoA') {
            steps {
                git 'https://github.com/JeremiahJhon/RepoA.git'
            }
        }

        stage('Generate Config') {
            steps {
                bat 'doxygen -g Doxyfile'
            }
        }

        stage('Modify Config') {
            steps {
                powershell '''
                (Get-Content Doxyfile) `
                  -replace '^INPUT .*', 'INPUT = src' `
                  -replace '^GENERATE_HTML .*', 'GENERATE_HTML = YES' `
                  -replace '^GENERATE_LATEX .*', 'GENERATE_LATEX = NO' |
                Set-Content Doxyfile

                Add-Content Doxyfile "WARN_LOGFILE = warnings.log"
                '''
            }
        }

        stage('Run Doxygen') {
            steps {
                bat 'doxygen Doxyfile'
            }
        }

        stage('Clone RepoC') {
            steps {
                git 'https://github.com/JeremiahJhon/RepoC.git'
            }
        }

        stage('Run Parser') {
            steps {
                bat 'python parser.py warnings.log'
            }
        }

        stage('Archive Output') {
            steps {
                archiveArtifacts artifacts: '*.csv'
            }
        }
    }
}