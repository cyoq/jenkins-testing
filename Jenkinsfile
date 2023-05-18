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
        stage('deploy-to-dev') {
            steps {
                script{
                    deploy("dev", 7001)
                }
            }
        }
        stage('tests-on-dev') {
            steps {
                script{
                    test("dev")
                }
            }
        }
        stage('deploy-to-staging') {
            steps {
                script{
                    deploy("staging", 7002)
                }
            }
        }
        stage('tests-on-staging') {
            steps {
                script{
                    test("staging")
                }
            }
        }
        stage('deploy-to-preprod') {
            steps {
                script{
                    deploy("preprod", 7003)
                }
            }
        }
        stage('tests-on-preprod') {
            steps {
                script{
                    test("preprod")
                }
            }
        }
        stage('deploy-to-prod') {
            steps {
                script{
                    deploy("prod", 7004)
                }
            }
        }
        stage('tests-on-prod') {
            steps {
                script{
                    test("prod")
                }
            }
        }
    }
}


def installPipDeps() {
    echo "[*] Installing all required pip dependencies."
    dir ('pg') {
        git branch: 'main', url: 'https://github.com/mtararujs/python-greetings.git'
    }
    powershell '''
        if ((Test-Path pg/requirements.txt) -eq $false) { 
            Write-Host "requirements.txt was not found. Exiting...";
            exit 1;
        }
    '''
    powershell 'pip install -r pg/requirements.txt'
}

def deploy(String environment, int port) {
    echo "[*] Deployment to ${environment} has started..."
    powershell "pm2 delete greetings-app-${environment}; exit 0"
    // Needs double escape for powershell to work
    powershell "pm2 start pg/app.py --name greetings-app-${environment} -- -- --port ${port}"
}

def test(String environment) {
    echo "[*] Testing on ${environment} has started..."
    git branch: 'main', url: 'https://github.com/mtararujs/course-js-api-framework.git'
    powershell "npm install"
    powershell "npm run greetings greetings_${environment}"
}