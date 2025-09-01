pipeline {
    agent any

    environment {
        DOTNET_PATH = '"C:\\Program Files\\dotnet\\dotnet.exe"'
        MSBUILD_PATH = '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\MSBuild\\Current\\Bin\\MSBuild.exe"'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/deepakchr/Test_exe.git',
                        credentialsId: 'fe758c53-d405-474d-8483-b56ee20492e2'
                    ]]
                ])
            }
        }

        stage('Restore NuGet Packages') {
            steps {
                bat "${DOTNET_PATH} restore test.sln"
            }
        }

        stage('Build Solution') {
            steps {
                bat "${MSBUILD_PATH} test.sln /p:Configuration=Release /t:Rebuild"
            }
        }

        stage('Archive Artifact') {
            steps {
                // Updated path to match your workspace structure
                archiveArtifacts 'ConsoleApp1/bin/Test/Test/bin/Debug/net8.0/*.exe'
            }
        }

        stage('Cleanup') {
            steps {
                // Delete the archived EXE after archiving
                bat 'del /Q ConsoleApp1\\bin\\Test\\Test\\bin\\Debug\\net8.0\\*.exe'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
