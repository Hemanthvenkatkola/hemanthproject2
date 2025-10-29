pipeline {
    agent any
    environment {
    PATH = "/usr/local/src/apache-maven/bin:$PATH"
    }
    stages {
        stage('GitHub Clone') {
            steps {
                git branch: 'project02', credentialsId: 'b8649f09-a724-435e-bdbb-31f5068cbb81', url: 'https://github.com/Chey2266/project02.git''
            }
        }
        stage('Build Maven') {
            steps {
                sh "mvn clean install package"
            }
        }
        stage('Deploy Tomcat') {
            steps {
                deploy adapters: [tomcat9(alternativeDeploymentContext: '',  credentialsId: 'c0264521-4691-4a27-b899-e1e134782a59',
                path: '',
                url: 'http://13.201.117.230:8080/')],
                contextPath: 'project02',
                war: '**/*.war'
            }
        }
        stage('add files') {
            steps {
                sh 'git add Jenkinsfile'
            }
        }
        
    }
    post {
        success {
            emailext to: "cheym2266@gmail.com",
            recipientProviders: [developers()],
            subject: "jenkins Pipe :${currentBuild.currentResult}: ${env.JOB_NAME}",
            body: "${currentBuild.currentResult}: Job ${env.JOB_NAME}\n More Info can be found here: ${env.BUILD_URL}",

            attachLog: true

            slackSend message: "Build deployed successfully - Job ${env.JOB_NAME}\n More Info can be found here: ${env.BUILD_URL}"
        }
    }
}
