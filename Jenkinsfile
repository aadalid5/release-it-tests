pipeline {
    agent any
    
    tools { nodejs "node" }
    
    environment {
        FOO = "initial FOO value"
    }

    stages {
        stage("install"){
            steps{
                sh " npm install"
            }
        }
        stage('git remote'){
            steps {
                sshagent(["github-key-a-id"]){
                    script {
                        sh 'git remote set-url origin  git@github.com:aadalid5/release-it-demo.git'
                        // sh 'git push origin HEAD:main && git push --tags'
                    }
            }
            }
        }

        stage('post release-bump version'){
            steps{
                sshagent(["github-key-a-id"]){
                    script {
                        sh "git fetch"
                        // sh "git checkout main"
                        newVersion = sh(script: "npm version patch --commit-hooks=false -m 'bump version to %s'", returnStdout: true)
                        sh "git push origin main --no-verify && git push --tags --no-verify"
                    }
                }
                script{
                    sh "npm -v"
                    sh "npm run release"
                }
            }
        }
    }
}
