pipeline {
    agent any

    stages {
        stage('MVNV_Validate') {
            steps {
                echo 'Validating..'
		sh 'mvn compile'
            }
        }
        stage('MVN_Test') {
            steps {
                echo 'Testing..'
		sh 'mvn test'
            }
        }
        stage('Sonar_Analysis') {
            steps {
                echo 'Performing Sonar Analysis....'
            	sh 'mvn sonar:sonar \
  -Dsonar.host.url=http://3.90.105.90:9000 \
  -Dsonar.login=ccf1672d9d5e562264b20f6d76d6597ccaba8058'
		}
        }
        stage('Nexus_Deploy') {
            steps {
                echo 'Deploying artifact to Nexus....'
            	sh 'mvn deploy'
            }
        }
        stage('Tomcat_Deploy') {
            steps {
                echo 'Deploying application from Nexus to tocmat....'
            	script {	
			def mavenPom = readMavenPom file: 'pom.xml'
			sh 'wget --user=admin --password=admin123 "http://3.90.105.90:8081/nexus/service/local/repositories/releases/content/com/web/cal/WebAppCal/${mavenPom.version}/WebAppCal-${mavenPom.version}.war"'
            		sh 'sudo cp WebAppCal-${mavenPom.version}.war /home/centos/apache-tomcat-7.0.94/webapps/'
		}
            }
        }
    }
}
