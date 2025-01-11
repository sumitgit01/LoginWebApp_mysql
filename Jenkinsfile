pipeline {
    agent {
        label 'seh_01'
    }
    stages {
        stage("git checkout") {
            steps { 
                git 'https://github.com/sumitgit01/LoginWebApp_mysql.git'
            }
        }
        stage("build") {
            steps {
                sh """
                    echo '**Building the project**'
                    pwd
                    ls -alrt
                    maven clean package
                    echo '**Build completed**'
                """
            }
        }
    }
}