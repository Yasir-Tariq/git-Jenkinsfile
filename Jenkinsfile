def tag_commit
def version
def remote_tag
pipeline {
  agent any
  stages {
    stage ("checkout to Jenkins_Git_Tagging") {
        steps {
            sshagent (credentials: ['ssh-key']) {
                git url: 'git@github.com:Yasir-Tariq/Jenkins_Git_Tagging.git'
            }
        }
    }
    stage ("echo version") {
        steps {
            sshagent (credentials: ['ssh-key']) {
                script {
                    version = sh(script: "head -1 version.txt || :", , returnStdout: true).trim() //getting the version string from the text file
                    sh "echo ${version}"
                    // tag_commit = sh(script: "echo \$(git rev-list -n 1 ${version}) || :", , returnStdout: true).trim() //getting the commit to pointer id of the tag
                    // remote_tag = sh(script: "echo \$(git ls-remote --tags origin | grep ${version}) || :", , returnStdout: true).trim() //finding the tag in the remotely pushed tags
                    // if (tag_commit.isEmpty() == false && remote_tag.isEmpty() == false) { //checking if the values are existing
                    //     if (tag_commit != GIT_COMMIT) {
                    //         error('Tag version mentioned is behind the latest commit OR mentioned version already exists')
                    //     }
                    //     else {
                    //         echo "Tag is in accordance with latest commit"
                    //     }
                    // }
                    // else {
                    //     echo "Tag doesnot exist remotely, adding new"
                    //     sh "git tag -a ${version} -m 'adding new tag'"
                    //     sh "git push -f origin ${version}"
                    // }
                }
            }
        }
    }
  }
}