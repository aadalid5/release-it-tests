pipeline {
    agent any
    
    tools { nodejs "node" }
    
    environment {
        FOO = "initial FOO value"
    }

    stages {
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
                        sh "git checkout main"
                        sh "git pull"
                        sh "git reset --hard HEAD"
                        newVersion = sh(script: "npm version patch --commit-hooks=false -m 'bump version to %s'", returnStdout: true)
                        sh "git push --no-verify && git push --tags --no-verify"
                        
                            sh "export GITHUB_TOKEN=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"
                            sh " echo $GITHUB_TOKEN"
                            sh "npx release-it@14.14.3 --no-npm --no-git --no-increment --github.release --ci"
                    }
                }
            }
        }
    }
}
