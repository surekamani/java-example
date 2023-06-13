
pipeline{
        agent {label 'docker'}
        environment {
		DOCKER_LOGIN_CREDENTIALS=credentials('docker')
	    }
    stages {
        stage ('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/surekamani/java-example.git'
            }
        }
        stage('Build') {
            parallel {
                stage ('maven'){
                    steps {
                        sh 'mvn clean install'
                    }
                }
                stage ('image'){
                    steps {
                        script {
                            sh 'docker build --tag surekamani/java:$BUILD_NUMBER .'
                         }
                    }
                }
            }
        }
        stage('Push') {
            steps {
                    sh 'echo $DOCKER_LOGIN_CREDENTIALS_PSW | docker login -u $DOCKER_LOGIN_CREDENTIALS_USR --password-stdin'
                    sh 'docker push surekamani/java:$BUILD_NUMBER'
                }
        }
        stage('Deploy') {
            steps {
                    sh 'kubectl apply -f deployment.yaml'
                   
            }
        }
    }
}
