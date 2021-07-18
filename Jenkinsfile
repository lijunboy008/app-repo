pipeline {
    agent {
        label 'gitops-jenkins-jenkins-slave'
    }
    stages {
        stage('Get Source') {
            steps {
                echo "1.Clone Repo Stage"
                git credentialsId: 'c6a3a5d9-2f30-4f1e-9202-10f0e3fe1d04', url: 'https://github.com/lijunboy008/app-repo'
                script {
                    build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    repo_name = '917958955567.dkr.ecr.ap-southeast-1.amazonaws.com'
                    app_name = 'lijun-app'
                }
            }
        }
        stage('Build Image') {
            steps {
                echo "2.Build Docker Image Stage"
                sh "docker build --network host -t ${repo_name}/${app_name}:latest ."
                sh "docker tag ${repo_name}/${app_name}:latest ${repo_name}/${app_name}:${build_tag}"
            }

        }
        stage('Push Image') {
            steps {
                echo "3.Push Docker Image Stage"
                withDockerRegistry([credentialsId: 'ecr:ap-southeast-1:cfecdff8-648a-416e-a977-bcaac897ee2c', url: 'https://917958955567.dkr.ecr.ap-southeast-1.amazonaws.com/lijun-app'])  {
                    sh "docker push ${repo_name}/${app_name}:latest"
                    sh "docker push ${repo_name}/${app_name}:${build_tag}"
                }
            }
        }
    }
}
