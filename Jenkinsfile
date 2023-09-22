pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar'
            }
        }

      stage('Unit Tests - JUnit and JaCoCo') {
	    	steps {
			sh "mvn test"
	    	}
		post {
			always {
				junit 'target/surefire-reports/*.xml'
				jacoco execPattern: 'target/jacoco.exec'
			}
		}
        }
	stage('Docker build and push') {
			steps {
				withDockerRegistry([credentialsId: 'dockercred', url: 'docker.io']) {
					sh 'printenv'
					sh 'docker build -t <docker-hub-user>/numeric-app:""$GIT_COMMIT"" .'
					sh 'docker push <docker-hub-user>/numeric-app:""$GIT_COMMIT""'
				}
			}
		}
  }
}
