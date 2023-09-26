pipeline{   
    agent none

    tools{
        jdk 'myjava'
        maven 'mymaven'
    }
    parameters{
        string(name:'ENV',defaultValue:"Test",description:"This is sample application")
        booleanParam(name: 'executeTest', defaultValue: true, description: 'Decide to run test')
        choice(name: 'APPVERSION', choices: ['1.1', '1.2', '1.3'], description: '') 
    }
    stages{
        stage('Compile'){
            agent any
            steps{
                script{
                    echo "Compile the code for the Application${params.ENV}"
                    sh "mvn compile"
                }
            }
        }
        stage('UnitTest'){
            agent any
            when{
                expression{
                    params.executeTest == true
                }
            }
            steps{
                script{
                    echo "Run the Unit test  cases"
                    sh "mvn test"
                }
            }
            post{
                always{
                    junit 'target/surefire/*.xml'
                }
            }
        }
        stage('Package'){
            agent {label 'linux_slave'}
            steps{
                script{
                    echo "Package the code"
                    echo "Packaging the code version ${params.APPVERSION}"
                    sh "mvn package"
                }
            }
        }
        stage('Deploy'){
            agent any
            input{
                message "Select the version of the package"
                ok "Version selected "
                parameters{
                    choice(name:'NEWVERSION',choices:['3','4','5'])
                }
            }
            steps{
                script{
                    echo "Deploying the packaged code"
                    echo "Deploying code version ${params.NEWVERSION}"
                }
            }
        }
    }
}