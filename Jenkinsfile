pipeline {
    agent any
    
    stages {
        stage('Cloning Image') {
          steps {
	      echo "start stage - Cloning Image!!!!!!!!"
	      withCredentials([string(credentialsId: 'sspr_4', variable: 'sspr4')]) {
                    bat "docker login --username aln505 --password ${sspr4}"
                    bat 'docker pull aln505/sspr4:latest'
                }
          }
        }
        stage('Build') {

			steps {
				echo "start stage - Build!!!!"
				bat 'docker build -t aln505/sspr4:latest .'
			}
		}
        stage('Test') {
            steps {
		                echo "start stage - Test!!!!!"
				bat 'FOR /F "tokens=*" %%i IN (\'docker ps -a -q\') DO docker stop %%i'
				bat 'docker rm "test_sspr"'
				bat 'docker run -d --name "test_sspr" aln505/sspr4:latest bash'
				bat 'docker exec "test_sspr" sh -c "dotnet vstest TestProject.dll"'
				bat 'docker stop "test_sspr"'
            }
        }

        stage("Push Image To Docker Hub") {
            steps {
		echo "start stage - Push To Docker Hub!!!!"
                withCredentials([string(credentialsId: 'sspr_4', variable: 'sspr4')]) {
                    bat "docker login --username aln505 --password ${sspr4}"
                    bat 'docker push aln505/sspr4:latest'
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
