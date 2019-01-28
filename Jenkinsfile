pipeline {

    agent any
	// agent { label 'slave' }

    tools {
        ...
        gradle 'gradle-4.6'
        ...
    }
    ...
	
    stages {
        stage('cpd - Duplicated Code Analysis') {
            steps {
                sh "gradle -b <gradle config path>/build.gradle cpdCheck --info"
            }
        }
        stage('pmd - Style Analysis') {
            steps {
                sh "gradle -b <gradle config path>/build.gradle pmdMain --info"
            }
        }
        stage('findbugs - Bugs Analysis') {
            steps {
                sh "gradle -b <gradle config path>/build.gradle findbugsMain --info"
            }
        }
        stage('jacoco - Code Coverage Analysis') {
            steps {
                sh "gradle -b <gradle config path>/build.gradle clean stagetest test jacocoTestReport --info"
            }
        }
        ...

		stage('Retrying') {
			steps {
				retry(3) {
					// Try to execute successfully the below command up to 3 times, otherwhise exit with error code.
					echo 'Retrying...'
				}
			}
		}
		stage('Waiting for a timeout') {
			steps {
				timeout(time: 1, unit: 'MINUTES') {
					// Waiting for finish up to 1 minute, otherwhise exit with error code.
					echo 'Waiting 1 minute...'
				}
			}
		}
    }
	
    post {
        always {
            recordIssues enabledForFailure: true,
                tools: [
                    pmdParser(canComputeNew: false, pattern: '<reports path>/pmd/pmd.xml'),
                    spotBugs(canComputeNew: false, isRankActivated: true, pattern: '<reports path>/findbugs/findbugs.xml'),
                    cpd(canComputeNew: false, pattern: '<reports path>/cpd/cpd.xml')
                    ]
                jacoco(
                    execPattern: '<microservice path>/build/jacoco/test.exec',
                    classPattern: '<microservice path>/build/classes/',
                    exclusionPattern: '<microservice path>/**/test/',
                    sourcePattern: '<microservice path>/src/main/'
                )
                archive '<microservice path>/build/jacoco/**/*.xml'
                publishHTML(
                    [allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: '<microservice path>/build/reports/tests/test/', reportFiles: 'index.html', reportName:'Test Report', reportTitles: ''])
                    junit '<microservice path>/build/test-results/**/*.xml'
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
