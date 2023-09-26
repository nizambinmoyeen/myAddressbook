pipeline{   
    agent any
    parameters{
        string(name:'ENV',defaultValue:"Test",description:"This is sample application")
        booleanParam(name: 'executeTest', defaultValue: true, description: 'Decide to run test')
        choice(name: 'APPVERSION', choices: ['1.1', '1.2', '1.3'], description: '') 
    }
    stages{
        stage('Compile'){
            steps{
                script{
                    echo "Compile the code"
                }
            }
        }
        stage('UnitTest'){
            when{
                expression{
                    params.executeTest == true
                }
            }
            steps{
                script{
                    echo "Run the Unit test  cases"
                }
            }
        }
        stage('Package'){
            input{
                message "Select the version"
                ok "Version selected "
                parameters{
                    choice{name:"NEW_VERSION",choices[3,4,5]}
                }
            }
            steps{
                script{
                    echo "Package the code"
                    echo "Packaging the code version ${params.APPVERSION}"
                }
            }
        }
    }
}