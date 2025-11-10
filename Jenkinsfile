pipeline {
	agent any
	environment{
		NETLIFY_SITE_ID = '7dcf00f4-bf71-48ea-be16-8284f26a2714'
	}
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
				echo "deploying to production. Site ID :$NETLIFY_SITE_ID"
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