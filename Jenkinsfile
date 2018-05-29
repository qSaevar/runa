pipeline {
    stages {
        stage('Test') {
            steps {
                sh 'python --version'
                sh 'pwd'
                sh 'ls -lah'
                sh 'coverage run --source="./src/qc" -m unittest discover -s src/tests -t src -v'
                sh 'coverage report -m'
                sh 'coverage xml'
            }
        }
        stage('Collect') {
            steps {
                step([$class: 'CoberturaPublisher', coberturaReportFile: 'coverage.xml'])
            }
        }
    }
}
