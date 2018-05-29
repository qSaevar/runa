pipeline{
    agent {
        docker {
            image 'jfloff/alpine-python'
            args '-v pip_cache:/var/pip_cache'
        }
    }
    stages {
    stage('build') {

        steps {
            sh 'python --version'
        }
    }
    }
}

