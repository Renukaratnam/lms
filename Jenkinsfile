pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'cd webapp && npm install && npm run build'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'cd webapp && sudo docker container run --rm -e SONAR_HOST_URL="http://43.204.228.248:9000" -e SONAR_LOGIN="sqp_a67c9490f84ecbe007c0f0ddd3e55a5556f329a5" --volume ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
            }
        }
        stage('Releasing') {
            steps {
                echo 'Releasing Nexus'
                sh 'cd webapp && zip dist-${BUILD_NUMBER}.zip -r dist/'
                sh 'cd webapp && curl -v -u admin:chinni --upload-file dist-${BUILD_NUMBER}.zip http://43.204.228.248:8081//repository/lms/'
                sh 'cd'
                sh 'curl -u admin:gopal -X GET http://43.204.228.248:8081/repository/lms/dist-${BUILD_NUMBER}.zip --output dist-${BUILD_NUMBER}.zip'
                sh 'unzip dist-${BUILD_NUMBER}.zip dist'
                sh 'sudo docker container run -dt --name original -p 8082:80 --volume /home/ubuntu/dist:/usr/share/nginx/html nginx'



            }
        }
    }
}
