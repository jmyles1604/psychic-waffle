pipeline{
    agent any
    stages{
        stage("make a directory"){
            steps{
                sh "mkdir ~/jenkins-folder"
            }
        }
        stage("add a file to the new directory"){
            steps{
                sh "touch ~/jenkins-folder/file1.txt"
            }
        }
    }

}