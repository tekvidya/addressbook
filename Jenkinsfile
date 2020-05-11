pipeline {
	agent any
	stages {
		stage ("Checkout") {
			steps {
				checkout([$class: 'GitSCM',	
					 branches: [[name: '*/master']],
					 userRemoteConfigs: [[url: 'https://github.com/tekvidya/addressbook.git']]])
			}
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
			post {
				always {
					pmd canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/target/pmd.xml', unHealthy: ''
				}
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
			post {
				always {
					cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
				}
			}
		}
		stage ("Package") {
			steps {
				sh "mvn package"
			}
		}
	}
}
