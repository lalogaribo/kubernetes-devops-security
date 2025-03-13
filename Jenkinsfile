pipeline {
  agent any
  tools {
    maven 'maven3'
  }
  stages {
      stage('Build Artifact Maven') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco execPattern: 'target/jacoco.exec'
                }
            }
        }
        // https://registry.hub.docker.com/
        stage('Docker Build & Push') {
            steps {
              withDockerRegistry(credentialsId: 'docker-registry', url: 'https://index.docker.io/v1/') {
                    sh 'printenv'
                    sh 'docker build -t egaribo/numeric-app:""$GIT_COMMIT"" .'
                    sh 'docker push egaribo/numeric-app:""$GIT_COMMIT""'
                }
            }
        }
        
    }
}
