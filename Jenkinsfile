def tag_commit
def version
def remote_tag
def commit_id
pipeline {
  agent any
  parameters {
        string(name: 'checkout_url', defaultValue: 'git@github.com:Yasir-Tariq/Jenkins_Git_Tagging.git', description: 'url for the repository')
        string(name: 'credentials', defaultValue: 'ssh-key', description: 'credentials for ssh connection')
        string(name: 'branch_name', defaultValue: 'master', description: 'Select branck to checkout')
    }
  stages {
    stage ("checkout to repository to access versions.txt") {
        steps {
            script {
                sshagent (credentials: ['ssh-key']) {
                git branch: "${params.branch_name}",
                credentialsId: "${params.credentials}",
                url: "${params.checkout_url}"
                //getting the latest commit id of the repository where versions.txt exists
                commit_id = sh(script: "git ls-remote git@github.com:Yasir-Tariq/Jenkins_Git_Tagging.git refs/heads/master | head -c 40", , returnStdout: true).trim() //getting the version string from the text file
                version = sh(script: "head -1 version.txt || :", , returnStdout: true).trim() //getting the version string from the text file
                tag_commit = sh(script: "echo \$(git rev-list -n 1 ${version}) || :", , returnStdout: true).trim() //getting the commit to pointer id of the tag
                remote_tag = sh(script: "echo \$(git ls-remote --tags origin | grep ${version}) || :", , returnStdout: true).trim() //finding the tag in the remotely pushed tags
                if (tag_commit.isEmpty() == false && remote_tag.isEmpty() == false) { //checking if the values are existing
                    if (tag_commit != commit_id) {
                        error('Tag version mentioned is behind the latest commit OR mentioned version already exists')
                    }
                    else {
                        echo "Tag is in accordance with latest commit"
                    }
                }
                else {
                    echo "Tag doesnot exist remotely, adding new"
                    sh "git tag -a ${version} -m 'adding new tag'"
                    sh "git push -f origin ${version}"
                }
            }
            }
            
        }
    }
  }
}