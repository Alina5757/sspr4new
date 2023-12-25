pipeline {
    agent any
    
    stages {
        stage('Cloning Git') {
          steps {
	      echo "start stage - Cloning Git!!!!!!!!"
              git([url: 'https://github.com/Alina5757/sspr4new.git', branch: 'main'])
          }
        }
        stage('Build') {

			steps {
				echo "start stage - Build!!!!"
				bat 'docker build -t Alina5757/sspr4:latest .'
			}
		}
        stage('Test') {
            steps {
		                echo "start stage - Test!!!!!"
				bat 'FOR /F "tokens=*" %%i IN (\'docker ps -a -q\') DO docker stop %%i'
				bat 'docker rm "test_sspr"'
				bat 'docker run -d --name "test_sspr" Alina5757/sspr4:latest bash'
				bat 'docker exec "test_sspr" sh -c "dotnet vstest TestProject.dll"'
				bat 'docker stop "test_sspr"'
            }
        }

        stage("Push Image To Docker Hub") {
            steps {
		echo "start stage - Push To Git!!!!"
                withCredentials([string(credentialsId: 'dockerhub', variable: 'sspr4')]) {
                    bat "docker login --username Alina5757 --password ${sspr4}"
                    bat 'docker push Alina5757/sspr4:latest'
                }
            }
        }
    }
    post {
		always {
			script {
				node {
					bat 'docker logout'
				}
            }
		}
	}
}
