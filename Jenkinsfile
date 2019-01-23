pipeline {

    agent any
	
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
				sh "C:/ProgramData/chocolatey/bin/gradle.exe -b ./build.gradle clean build"
				sh "C:/ProgramData/chocolatey/bin/gradle.exe -b ./build.gradle cpdCheck --info"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
	
    post {
        always {
            echo 'This will always run'
			//recordIssues enabledForFailure: true, tool: java(pattern: '*.xml'), filters: [includeFile('MyFile.*.java'), excludeCategory('WHITESPACE')]
																				// filters: Chido di visualizzare gli errori solo per i files indicati tramite filtri
			recordIssues enabledForFailure: true, tool: mavenConsole()
			recordIssues enabledForFailure: true, tool: java()
			recordIssues enabledForFailure: true, tool: javaDoc()
			recordIssues enabledForFailure: true, tool: checkStyle()
			recordIssues enabledForFailure: true, tool: spotBugs()
			recordIssues enabledForFailure: true, tool: cpd()
			recordIssues enabledForFailure: true, tool: pmd()
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
