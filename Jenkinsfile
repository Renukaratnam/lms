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
                sh 'cd webapp && sudo docker container run --rm -e SONAR_HOST_URL="http://13.126.112.116:9000" -e SONAR_LOGIN="sqp_0931487430d5e6d524fe72985648d324d6258053" --volume ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms-project/'
            }
        }
        stage('Releasing') {
            steps {
                echo 'Releasing Nexus'
                sh 'cd webapp && zip dist-${BUILD_NUMBER}.zip -r dist/'
                sh 'cd webapp && curl -v -u admin:chinni --upload-file dist-${BUILD_NUMBER}.zip http://13.126.112.116:8081//repository/lms/'
                sh 'cd'
                sh 'curl -u admin:gopal -X GET http://13.126.112.116:8081/repository/lms/dist-${BUILD_NUMBER}.zip --output dist-${BUILD_NUMBER}.zip'
                sh 'unzip dist-${BUILD_NUMBER}.zip dist'
                sh 'sudo docker container run -dt --name original -p 8082:80 --volume /home/ubuntu/dist:/usr/share/nginx/html nginx'



            }
        }
    }
}
