pipeline {
    agent any
     tools {nodejs "node"}
      /*environment {
         PATH='/usr/local/bin:/usr/bin:/bin'
      }*/
    stages{
    stage('Checkout') {
        //disable to recycle workspace data to save time/bandwidth
         steps{
        deleteDir()
        checkout scm
         }
        //enable for commit id in build number
        //env.git_commit_id = sh returnStdout: true, script: 'git rev-parse HEAD'
        //env.git_commit_id_short = env.git_commit_id.take(7)
        //currentBuild.displayName = "#${currentBuild.number}-${env.git_commit_id_short}"
    }

    stage('NPM Install') {
        /*withEnv(["NPM_CONFIG_LOGLEVEL=warn"]) {*/
        steps{ 
            sh 'npm install'
            sh 'npm install -g @angular/cli@latest'
        }
        /*}*/
    }

    stage('Build') {
         steps{
        milestone(20)
        sh 'ng -- build --prod --aot --sm --progress=false'
         }
    }

    stage('Archive') {
         steps{
        sh 'tar -cvzf dist.tar.gz --strip-components=1 dist'
        archive 'dist.tar.gz'
         }
    }

    stage('Deploy') {
         steps{
        milestone(20)
        echo "Deploying..."
         }
    }
    }
}
