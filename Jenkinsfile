pipeline {
    agent any

    environment {
        SOLUTION_NAME = 'test.sln' // Your solution file
        PROJECT_DIR = 'ConsoleApp1' // Your project folder
        BUILD_CONFIG = 'Release'
        EXE_PATH = 'ConsoleApp1\\bin\\Test' // Path where .exe is located
        MSBUILD_PATH = 'C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\MSBuild\\Current\\Bin\\MSBuild.exe'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/deepakchr/Test_exe.git'
            }
        }

        stage('Restore NuGet Packages') {
            steps {
                bat "\"C:\\Program Files\\dotnet\\dotnet.exe\" restore ${SOLUTION_NAME}"
            }
        }

        stage('Build Solution') {
            steps {
                bat "\"${MSBUILD_PATH}\" ${SOLUTION_NAME} /p:Configuration=${BUILD_CONFIG} /t:Rebuild"
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: "${EXE_PATH}\\*.exe", fingerprint: true
            }
        }

        stage('Test Run') {
            steps {
                bat "\"${EXE_PATH}\\ConsoleApp1.exe\""
            }
        }
    }

    post {
        always {
            echo "Cleaning up workspace..."
            bat "del /Q ${EXE_PATH}\\*.exe"
        }

        success {
            echo "Build, archive, and test run completed successfully!"
        }

        failure {
            echo "Pipeline failed. Check the logs for errors."
        }
    }
}
