pipeline {
    agent any

    stages {
        stage('Clone RepoA') {
            steps {
                git 'https://github.com/JeremiahJhon/RepoA.git'
            }
        }

        stage('Generate Doxygen Config') {
            steps {
                sh 'doxygen -g Doxyfile'
            }
        }

        stage('Modify Config') {
            steps {
                sh '''
                sed -i 's|^INPUT .*|INPUT = src|' Doxyfile
                sed -i 's|^GENERATE_HTML .*|GENERATE_HTML = YES|' Doxyfile
                sed -i 's|^GENERATE_LATEX .*|GENERATE_LATEX = NO|' Doxyfile
                '''
            }
        }

        stage('Run Doxygen') {
            steps {
                sh 'doxygen Doxyfile'
            }
        }

        stage('Archive Docs') {
            steps {
                sh 'tar -czf doc.tar.gz html'
                archiveArtifacts artifacts: 'doc.tar.gz'
            }
        }
    }
}