pipeline {
    agent {
        label 'master'
    }
    stages {
        stage('Prepare') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                echo 'Prepare'
				sh 'npm install'
            }
        }
        stage('Build') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                echo 'Build'				
				sh 'npm run build'
			}
        }
        stage('Static Analysis') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                echo 'Analyze' 
				sh 'npm run lint'
            }
        }
        stage('Unit Test') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                echo 'Test'
				sh 'npm run test'
            }
        }
        stage('e2e Test') {
            steps {             
                echo 'e2e Test'
				sh 'docker-compose -f docker-compose-e2e.yml build'
				sh 'docker-compose -f docker-compose-e2e.yml up -d frontend backend'
            }
            post {
                always {
                    echo 'Cleanup'
					sh 'docker-compose -f docker-compose-e2e.yml down --rmi=all -v'
                }
            }
        }
        stage('Deploy') {
            steps {                
                echo 'Deploy'
				sh 'docker-compose -f docker-compose.yml build'
				
            }
        }
    }
}
