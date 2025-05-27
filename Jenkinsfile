// Jenkinsfile
pipeline {
    agent any  // Run this pipeline on any available agent
    stages {
        stage('Checkout') { // Stage to checkout the code
            steps {
                git url: 'https://github.com/yukthip/mypythonapp.git', branch: 'master' // For local git repo. For remote: 'https://github.com/your_username/my_python_app.git'
                // If your default branch is 'master', change 'main' to 'master'
                echo 'Code checked out successfully!'
            }
        }

stage('Setup Python Environment') { // Stage to set up a virtual environment and install dependencies
            steps {
                sh 'python3 --version' // verify python3 is available
                sh 'python3 -m venv venv' // Create a virtual environment
                // For Windows agents, you might use: bat 'python -m venv venv'
                // And activate differently: bat 'venv\\Scripts\\activate'
                sh '. venv/bin/activate && pip install -r requirements.txt' // Activate and install (Linux/macOS)
                // Note: Activation in sh step is temporary for that step.
                // For more complex scenarios, plugins like "ShiningPanda" or using Docker agents are better.
                echo 'Python environment set up and dependencies installed.'
            }
        }

stage('Run Tests') { // Stage to run our Python unit tests
            steps {
                sh '. venv/bin/activate && python3 -m unittest discover -s . -p "test_*.py"'
                // This command discovers and runs tests in files named test_*.py
                echo 'Python tests executed.'
            }
        }

stage('Archive Artifacts') { // Optional: Stage to archive test results or other build artifacts
            steps {
                // If you had XML test reports (e.g., from pytest --junitxml=report.xml), you could archive them:
                // junit 'report.xml'
                archiveArtifacts artifacts: 'app.py, test_app.py', followSymlinks: false
                echo 'Relevant files archived.'
            }
        }
    }

post { // Actions to perform after the pipeline finishes
        always {
            echo 'Pipeline finished.'
            cleanWs() // Clean up the workspace
        }
        success {
            echo 'Pipeline executed successfully! Hooray! '
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
