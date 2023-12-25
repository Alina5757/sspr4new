pipeline {
    agent any
    
    stages {
        stage('Cloning Git') {
          steps {
              git([url: 'https://github.com/Alina5757/sspr4new.git', branch: 'main'])
          }
        }
        stage('Build') {

			steps {
				bat 'docker build -t aln505/sspr4:latest .'
			}
		}
        stage('Test') {
            steps {
				bat 'FOR /F "tokens=*" %%i IN (\'docker ps -a -q\') DO docker stop %%i'
				bat 'docker rm "test_sspr"'
				bat 'docker run -d --name "test_sspr" aln505/sspr4:latest bash'
				bat 'docker exec "test_sspr" sh -c "dotnet vstest TestProject.dll"'
				bat 'docker stop "test_sspr"'
            }
        }

        stage("Push Image To Docker Hub") {
            steps {
                withCredentials([string(credentialsId: 'aln505', variable: 'aln505')]) {
                    bat "docker login --username aln505 --password ${aln505}"
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
