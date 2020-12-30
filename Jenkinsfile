pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
       stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
               sshPublisher(publishers: [sshPublisherDesc(configName: '测试机', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '/opt/test.sh', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/opt/', remoteDirectorySDF: false, removePrefix: '/opt/jenkins-data/jobs/test/workspace/target/', sourceFiles: '/opt/jenkins-data/jobs/test/workspace/target/my-app-1.0-SNAPSHOT.jar')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
  }
}
