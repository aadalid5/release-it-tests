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
                        sh "git pull origin main"
                        sh "git checkout main"
                        sh "git reset --hard HEAD"
                        newVersion = sh(script: "npm version patch --commit-hooks=false -m 'bump version to %s'", returnStdout: true)
                        sh "git push -f --no-verify && git push --tags --no-verify"
                        sh "npm run release:notes"
                    }
                }
            }
        }
    }
}
