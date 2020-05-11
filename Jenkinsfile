pipeline {
	agent any
	stages {
	
		stage ("Checkout") {
			checkout([$class: 'GitSCM',	
					 branches: [[name: '*/master']],
					 userRemoteConfigs: [[url: 'https://github.com/tekvidya/addressbook.git']]])
		}
		stage("Compile") {
			steps {
				sh "mvn compile"
			}
		}
		
		stage ("Code Review") {
			steps {
				sh "mvn -P metrics pmd:pmd"
			}
		}
		
		stage ("Unit Test") {
			steps {
				sh "mvn test"
			}
		}
		stage ("Metric Test") {
			steps {
				sh "mvn cobertura:cobertura -Dcobertura.report.format=xml"
			}
		}
		stage ("Package") {
			steps {
				sh "mvn package"
			}
		}
	}
}
