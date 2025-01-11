pipeline {
    agent {
        label 'seh-login'
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
                    mvn clean package
                    echo '**Build completed**'
                """
            }
        }
    }
}