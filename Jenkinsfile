pipeline {
    agent none
    stages {
        stage('Clone Repo') {
            agent { label 'master'}
            steps {
                git credentialsId: 'GITHUB_CREDENTIALS',
                url: 'https://github.com/nawab312/jenkins-master-slave-java-app.git',
                branch: 'main'
                stash name: 'cloned-repo' //Stash the cloned repo for use in slave nodes
            }
        }
        stage('Build on Slave Node1') {
            agent { label 'slave-node-1' }
            steps {
                unstash 'cloned-repo' //Retrieve the stashed repo from master
                sh 'mvn clean package'
                archiveArtifacts artifacts: '**/target/myapp*.jar', allowEmptyArchive: false // Archive build artifacts
                stash name: 'build-artifacts' //Stash the build artifact for the next stages
            }

        }
        stage('Deploy on Slave Node2') {
            agent { label 'slave-node-2' }
            steps {
                unstash 'build-artifacts'
            }
        }
    }
}
