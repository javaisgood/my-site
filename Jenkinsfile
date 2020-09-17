pipeline {
    agent any
    parameters {
        string(name: 'AppName', defaultValue: 'jswdwsx/blog', description: 'this is the app name')
    }
    stages {
        stage('Build jar') {
            steps {
                sh 'mvn -B -DskipTests clean package'
                sh 'echo Build jar done!'
            }
        }
        stage('Remove image') {
            steps {
                script {
                    try {
                        sh "docker ps -a | grep ${params.AppName} | awk '{print \$1}'| xargs docker rm -f"
                        sh "docker rmi ${params.AppName}"
                        sh 'echo Remove image done!'
                    }
                    catch (exc) {
                        echo 'image do not exist!'
                    }
                    finally {
                        sh 'echo Remove image finally done!'
                    }
                }
            }
        }
        stage('Build image') {
            steps {
                sh 'mvn docker:build'
                sh 'echo Build image done!'
            }
        }
        stage('Deploy') {
            steps {
                sh "docker run -p 9000:8080 -d ${params.AppName}"
            }
        }
    }
}
