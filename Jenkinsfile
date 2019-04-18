node {
    checkout scm

    // stage('Frontend: Test and Build') {
    //     dir('frontend') {
    //         docker.image('trion/ng-cli-karma:7.3.8').inside { c ->
    //             sh 'npm install && npm install --only=dev'
    //             sh 'ng test --watch=false --browsers=ChromeHeadless'
    //             sh 'ng lint'
    //             sh 'ng build --output-path=./dist/out --configuration production'
    //         }
    //     }
    // }

    stage('Backend: Test and Build') {
        dir('backend') {
            docker.image('golang:1.12.4').inside('-e HOME=/tmp') { c ->
                sh 'go version'
                
                echo('Pulling dependencies')
                sh 'go get -u golang.org/x/lint/golint'

                echo('Vetting')
                sh 'go vet'

                echo('Linting')
                sh 'golint -set_exit_status ./...'

                echo('Testing')
                sh 'go test'

                echo('Bulding')
                sh 'go build'
            }
        }
    }
    
    stage('Frontend: Build, Tag and Push image') {
        def version = readFile('VERSION').trim()
        echo(String.format('Image-Name: frontend-image:%s-%s-%s', version, BRANCH_NAME, BUILD_NUMBER))
    }

    stage('Backend: Build, Tag and Push image') {
        def version = readFile('VERSION').trim()
        echo(String.format('Image-Name: backend-image:%s-%s-%s', version, BRANCH_NAME, BUILD_NUMBER))
    }
}