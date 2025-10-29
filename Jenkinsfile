pipeline {
    agent any
    environment {
    PATH = "/usr/local/src/apache-maven/bin:$PATH"
    }
    stages {
        stage('GitHub Clone') {
            steps {
                git branch: 'hemanthproject2', url: 'https://github.com/Hemanthvenkatkola/hemanthproject2.git'
            }
        }
        stage('Build Maven') {
            steps {
                sh "mvn clean install package"
            }
        }
        stage('Deploy Tomcat') {
            steps {
                deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcat', 
                path: '', 
                url: 'http://65.0.109.38:8080/')], 
                contextPath: 'hemanthproject2', 
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
            emailext to: "hemanthvenkat184763@gmail.com",
            recipientProviders: [developers()],
            subject: "jenkins Pipe :${currentBuild.currentResult}: ${env.JOB_NAME}",
            body: "${currentBuild.currentResult}: Job ${env.JOB_NAME}\n More Info can be found here: ${env.BUILD_URL}",

            attachLog: true

            slackSend message: "Build deployed successfully - Job ${env.JOB_NAME}\n More Info can be found here: ${env.BUILD_URL}"
        }
    }
}
