pipeline {
    agent any

    stages {
        stage('Package') {
			steps {
				bat 'mvn clean'
				bat 'mvn checkstyle:checkstyle'
				echo 'Package...'
                bat 'mvn package'
            }
        }
		stage('Analyse') {
			steps {
				echo 'Analyse...'
			}
        }
        stage('Publish'){
            steps{
                echo 'Publish...'
            }
        }
    }
	post{
		always{
			archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            junit '/**/*.xml'

            recordIssues enabledForFailure: true, tools: [mavenConsole(), java()]
			recordIssues enabledForFailure: true, tools: checkStyle()
			recordIssues enabledForFailure: true, tools: spotBugs()
			recordIssues enabledForFailure: true, tools: cpd(pattern: '**/target/cpd.xml')
			recordIssues enabledForFailure: true, tools: pmdParser(pattern: '**/target/pmd.xml')
        }
	}
}