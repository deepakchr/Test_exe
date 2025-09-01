pipeline {
    agent any

    environment {
        DOTNET_HOME = "C:\\Program Files\\dotnet"
        MSBUILD_PATH = "C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\MSBuild\\Current\\Bin\\MSBuild.exe"
    }

    stages {

        stage('Checkout SCM') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/master']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/deepakchr/Test_exe.git',
                        credentialsId: 'fe758c53-d405-474d-8483-b56ee20492e2'
                    ]]
                ])
            }
        }

        stage('Restore NuGet Packages') {
            steps {
                bat "\"${DOTNET_HOME}\\dotnet.exe\" restore test.sln"
            }
        }

        stage('Build Solution') {
            steps {
                bat "\"${MSBUILD_PATH}\" test.sln /p:Configuration=Release /t:Rebuild"
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Archive both exe and dll from Release folder
                archiveArtifacts artifacts: 'Test/bin/Release/net8.0/*.{exe,dll}', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
        success {
            echo 'Build and archive completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
