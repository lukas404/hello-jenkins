node {
    checkout scm

    stage('Build and test application') {
        dir('frontend/toolbox-cloud-portal') {
            docker.image('trion/ng-cli-karma:7.3.8').inside { c ->
                sh 'npm install && npm install --only=dev'
                sh 'ng test --watch=false --browsers=ChromeHeadless'
                sh 'ng lint'
                sh 'ng build --output-path=./dist/out --configuration production'
            }
        }
    }
    
    stage('Build, tag and push Docker image') {
        def version = readFile('VERSION').trim()
        echo(String.format("Image-Name: frontend-image:%s-%s-%s", version, BRANCH_NAME, BUILD_NUMBER))
    }
}