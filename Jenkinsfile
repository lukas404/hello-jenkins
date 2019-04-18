node {
    checkout scm

    stages('Frontend') {
        dir('frontend/toolbox-cloud-portal') {
            docker.image('trion/ng-cli-karma:7.3.8').inside { c ->
                stage('Installation') {
                    sh 'npm install && npm install --only=dev'
                }
                stage('Testing') {
                    sh 'ng test --watch=false --browsers=ChromeHeadless'
                }
                stage('Linting') {
                    sh 'ng lint'
                }
                stage('Building') {
                    sh 'ng build --output-path=./dist/out --configuration production'
                }
            }
        }
    }
    
    stage('Build, tag and push Docker image') {
        def version = readFile('VERSION').trim()
        echo(String.format("Image-Name: frontend-image:%s-%s-%s", version, BRANCH_NAME, BUILD_NUMBER))
    }
}