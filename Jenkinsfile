pipeline {
    agent any
    tools {
        jdk 'JDK17'
        nodejs 'nodeJS16'
    }
    environment {
        SCANNER_HOME = tool 'SonarQube Scanner'
        APP_NAME = "reddit-clone-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "kumbharap9"
        DOCKER_PASS = 'Kumbhar@9'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	
    }
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/Kumbharap9/a-reddit-clone.git'
            }
        }
        stage("Sonarqube Analysis") {
            steps {
                sonar-scanner \
  			-Dsonar.projectKey=new-Project \
  			-Dsonar.sources=. \
  			-Dsonar.host.url=http://65.0.102.72:9000 \
  			-Dsonar.login=sqp_81b833f0bdcbf6874a4406901124fe9fb8e0e6b1
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-qube-token'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
	 
    }
}
