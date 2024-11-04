pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/kubade123/data-engineering-pipeline.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install Python dependencies
                script {
                    docker.image('python:3.8').inside {
                        sh 'pip install -r scripts/requirements.txt'
                    }
                }
            }
        }
        stage('Run Tests') {
            steps {
                // Run unit tests
                script {
                    docker.image('python:3.8').inside {
                        sh 'python -m unittest discover -s scripts'
                    }
                }
            }
        }
        stage('Run ETL') {
            steps {
                // Run the ETL process
                script {
                    docker.image('python:3.8').inside {
                        sh 'python scripts/etl.py'
                    }
                }
            }
        }
    }
}

post {
  always {
    // Archive the generated CSV files
    archiveArtifacts artifacts: '**/*.csv', allowEmptyArchive: true
  }
}