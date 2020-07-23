pipeline {
    agent {
        docker {
            image 'maven:latest'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B clean package -Dmaven.test.skip=true'
            }
        }
    }
}
node('master') {
    stage('Build Image') {
           sh './jenkins/scripts/image.sh'
    }
    stage('deploy to k8s') {
        sh 'sed -i "s#{VERSION}#${BUILD_ID}#g" ./jenkins/scripts/java-demo.yaml'
        sh 'kubectl apply -f ./jenkins/scripts/prometheus-demo.yaml'
    }
}
