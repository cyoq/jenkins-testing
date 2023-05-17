pipeline {
    agent any
    triggers{ pollSCM('*/1 * * * *') }
    

    stages {
        stage('install-pip-deps') {
            steps {
                script{
                    installPipDeps()
                }
            }
        }
        // stage('deploy-to-dev') {
        //     steps {
        //         script{
        //             deploy("DEV", 1010)
        //         }
        //     }
        // }
        // stage('tests-on-dev') {
        //     steps {
        //         script{
        //             test("BOOKS", "DEV")
        //         }
        //     }
        // }
        // stage('deploy-to-staging') {
        //     steps {
        //         script{
        //             deploy("STG", 2020)
        //         }
        //     }
        // }
        // stage('tests-on-staging') {
        //     steps {
        //         script{
        //             test("BOOKS", "STG")
        //         }
        //     }
        // }
        // stage('deploy-to-preprod') {
        //     steps {
        //         script{
        //             deploy("PRD", 3030)
        //         }
        //     }
        // }
        // stage('tests-on-preprod') {
        //     steps {
        //         script{
        //             test("BOOKS", "PRD")
        //         }
        //     }
        // }
        // stage('deploy-to-prod') {
        //     steps {
        //         script{
        //             deploy("PRD", 3030)
        //         }
        //     }
        // }
        // stage('tests-on-prod') {
        //     steps {
        //         script{
        //             test("BOOKS", "PRD")
        //         }
        //     }
        // }
    }
}

// for windows: bat "npm.."
// for linux/macos: sh "npm .."

def installPipDeps() {
    echo "[*] Installing all required pip dependencies."
    powershell "ls"
    powershell "git clone https://github.com/mtararujs/python-greetings; cd python-greetings"
    powershell 'if ((Test-Path requirements.txt) -eq $false) { Write-Host "requirements.txt was not found. Exiting..."; exit 1; }'
    powershell 'pip install -r requirements.txt'
}

def deploy(String environment, int port){
    echo "Deployment to ${environment} has started.."
    sh "pm2 delete \"books-${environment}\""
    sh "pm2 start -n \"books-${environment}\" index.js -- ${port}"
}

def test(String test_set, String environment){
    echo "Testing ${test_set} test set on ${environment} has started.."
    sh "npm run ${test_set} ${test_set}_${environment}"
}