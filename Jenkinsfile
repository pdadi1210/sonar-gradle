pipeline {
    agent any
     triggers {
        pollSCM '* * * * *'
    }
    stages {
	    /*
        stage('SCM') {
            steps {
                git branch: 'master', credentialsId: 'pdadi1210', url: 'https://github.com/pdadi1210/sonar-gradle.git'
            }
        }*/
        stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarserver') {
					sh '''
					chmod 0755 ./gradlew
					./gradlew sonarqube
					'''
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true, credentialsId: 'sonar-gradle'
                }
            }
        }
    }
}
