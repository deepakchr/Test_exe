pipeline {
    agent any

    environment {
        MSBUILD_PATH = 'C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\MSBuild\\Current\\Bin\\MSBuild.exe'
        SOLUTION = 'test.sln'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/deepakchr/Test_exe.git'
            }
        }

        stage('Build') {
            steps {
                bat "\"${MSBUILD_PATH}\" ${SOLUTION} /p:Configuration=Release /t:Rebuild"
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'HelloWorldApp\\bin\\Release\\*.exe', fingerprint: true
            }
        }

        stage('Test Run') {
            steps {
                bat 'HelloWorldApp\\bin\\Release\\HelloWorldApp.exe'
            }
        }
    }
}
