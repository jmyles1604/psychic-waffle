pipeline {
    agent any

    options {
        timestamps()
        ansiColor('xterm')
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    parameters {
        string(
            name: 'USERNAME',
            defaultValue: 'John',
            description: 'Name used by coolScript.sh'
        )
    }

    stages {
        stage('Build Information') {
            steps {
                sh '''
                    echo "================================"
                    echo "Job: $JOB_NAME"
                    echo "Build: $BUILD_NUMBER"
                    echo "Workspace: $WORKSPACE"
                    echo "Host: $(hostname)"
                    echo "Date: $(date)"
                    echo "================================"
                '''
            }
        }

        stage('List Files') {
            steps {
                sh '''
                    echo "Listing all files, including hidden files"
                    ls -la
                '''
            }
        }

        stage('Create Script') {
            steps {
                sh '''
                    cat > coolScript.sh <<EOF
#!/bin/bash

echo "Hello, ${USERNAME}"
echo "Welcome to Jenkins build ${BUILD_NUMBER}"
echo "Running on: \$(hostname)"
echo "Current date: \$(date)"
EOF

                    chmod +x coolScript.sh
                '''
            }
        }

        stage('Validate Script') {
            steps {
                sh '''
                    echo "Checking script syntax"
                    bash -n coolScript.sh
                    echo "Script syntax is valid"
                '''
            }
        }

        stage('Run Script') {
            steps {
                sh '''
                    echo "Running coolScript.sh"
                    ./coolScript.sh | tee output.txt
                '''
            }
        }

        stage('Test Output') {
            steps {
                sh '''
                    grep "Hello" output.txt
                    grep "Jenkins build" output.txt
                    echo "All output tests passed"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully'
        }

        failure {
            echo '❌ Build failed'
        }

        always {
            archiveArtifacts artifacts: 'coolScript.sh, output.txt',
                             allowEmptyArchive: true,
                             fingerprint: true
        }
    }
}