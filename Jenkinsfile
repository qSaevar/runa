pipeline {
    agent {
        docker {
            image 'jfloff/alpine-python'
            args '-v pip_cache:/var/pip_cache'
        }
    }
    stages {
        stage('Test') {
            steps {
                sh 'python --version'
                sh 'pwd'
                sh 'ls -lah'
            }
        }
     stage('Deploy') {

      def environment = "Prod"
      def description = "Deploying my branch"
      def ref = scmVars.GIT_COMMIT
      def owner = "qSaevar"
      def repo = "runa"
      def deployURL = "https://api.github.com/repos/${owner}/${repo}/deployments"
      def deployBody = '{"ref": "' + ref +'","environment": "' + environment  +'","description": "' + description + '"}'

      // Create new Deployment using the GitHub Deployment API
      def response = httpRequest authentication: 'issc29-GH', httpMode: 'POST', requestBody: deployBody, responseHandle: 'STRING', url: deployURL
      if(response.status != 201) {
          error("Deployment API Create Failed: " + response.status)
      }

      // Get the ID of the GitHub Deployment just created
      def responseJson = readJSON text: response.content
      def id = responseJson.id
      if(id == "") {
          error("Could not extract id from Deployment response")
      }

      // Execute Deployment
      def deployStatus = sh returnStatus: true, script: 'echo deploy'

      // Record new Deployment Status based on output
      def result = (deployStatus) ? 'failure' : 'success'
      def deployStatusBody = '{"state": "' + result + '","target_url": "http://github.com/deploymentlogs"}'
      def deployStatusURL = "https://api.github.com/repos/${owner}/${repo}/deployments/${id}/statuses"
      def deployStatusResponse = httpRequest authentication: 'issc29-GH', httpMode: 'POST', requestBody: deployStatusBody , responseHandle: 'STRING', url: deployStatusURL
      if(deployStatusResponse.status != 201) {
        error("Deployment Status API Update Failed: " + deployStatusResponse.status)
      }
}

    }
}
