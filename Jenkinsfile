pipeline {
	agent any
	environment {
		NETLIFY_SITE_ID = '7dcf00f4-bf71-48ea-be16-8284f26a2714'
		NETLIFY_AUTH_TOKEN = credentials('netlify-token')
	}
	stages {
		stage('Install & Build') {
			agent {
				docker {
					image 'node:18-alpine'
					reuseNode true
				}
			}
			steps {
				writeFile file: 'netlify.toml', text: '''
				[build]
				command = ""
				publish = "build" '''
				sh '''
					npm ci
					npm install netlify-cli
					npm run build
					node_modules/.bin/netlify --version
					echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
					node_modules/.bin/netlify status
					node_modules/.bin/netlify deploy --dir=build --prod
       			 '''
			}
		}
		stage('Test') {
			agent {
				docker {
					image 'node:18-alpine'
					reuseNode true
				}
			}
			steps {
				sh '''
        test -f build/index.html
        npm test
        '''
			}
		}
	}
	post {
		always {
			junit 'test-results/junit.xml'
		}
	}
}