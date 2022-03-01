def gv

pipeline {
    agent any
    // enviornment {
//         NEW_VERSION = '1.3.0'
//         GIT_CREDENTIALS = 'AnchalGitCred'
//     }
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: 'Chose a Version')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')
    }
    stages {
        stage("init") {
            steps {
                echo "Building version ${NEW_VERSION}"
               // echo "Git Credentials are By Globle env vaiable :  ${NEW_VERSION}"
              //  echo "Git Credentials are By with credentials wrapper :  ${NEW_VERSION}"
                withCredentials([
                usernamePassword(credentials : 'AnchalGitCred', usernameVariable :USER, passwordVariable:PWD )
                ]) {
                    echo "User name is ${USER} and password is ${PWD}"
                }

                script {
                   gv = load "script.groovy" 
                }
            }
        }
        stage("build") {
            steps {
                script {
                    gv.buildApp()
                }
            }
        }
        stage("test") {
            when {
                expression {
                    params.executeTests
                }
            }
            steps {
                script {
                    gv.testApp()
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
        post{
            stage("After Execution of everythings..."){
                steps{
            always{
                echo 'This Post bolck and we are in Always Block...'
            }
            failure{
                echo 'This Post bolck and we are in failure Block...'
            }
            success{
                echo 'This Post bolck and we are in Success Block...'
            }
                }
            }
        }
    }   
}
