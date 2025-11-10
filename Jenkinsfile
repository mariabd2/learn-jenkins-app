pipeline {
	agent any
	stages {
		stage('build') {
			agent{
				docker{
					image 'node:18-alpine'
					reuseNode true
				}
			}
			steps {
				sh '''
				npm install netlify-cli
				node_modules/.bin/netlify --version
				'''
			}
		}
		stage('Test stage'){
			agent{
				docker{
					image 'node:18-alpine'
					reuseNode true
				}
			}
			steps{
				sh'''
				test -f build/index.html
				npm test
				'''
			}
		}
	}
	post{
		always{
			junit 'test-results/junit.xml'
		}
	}
}