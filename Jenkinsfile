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
					sh 'podman build -t <docker-hub-user>/numeric-app:""$GIT_COMMIT"" .'
					sh 'podman push <docker-hub-user>/numeric-app:""$GIT_COMMIT""'
				}
			}
		}
  }
}
