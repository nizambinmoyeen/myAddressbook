pipeline{   
    agent none

    tools{
        jdk 'myjava'
        maven 'mymaven'
    }
    environment{
        DEV_SERVER="ec2-user@172.31.14.121"
    }
    // parameters{
    //     string(name:'ENV',defaultValue:"Test",description:"This is sample application")
    //     booleanParam(name: 'executeTest', defaultValue: true, description: 'Decide to run test')
    //     choice(name: 'APPVERSION', choices: ['1.1', '1.2', '1.3'], description: '') 
    // }
    stages{
        stage('Compile'){
            agent any
            steps{
                script{
                    // echo "Compile the code for the Application${params.ENV}"
                    sh "mvn compile"
                }
            }
        }
        stage('UnitTest'){
            agent {label 'linux_slave'}
            // when{
            //     expression{
            //         params.executeTest == true
            //     }
            // }
            steps{
                script{
                    echo "Run the Unit test  cases"
                    sh "mvn test"
                }
            }
            post{
                always{
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Package'){
            agent any
            steps{
                    script{
                        sshagent(['DEV_SERVER_PACKAGING']) {
                            echo "Package the code"
                            // // echo "Packaging the code version ${params.APPVERSION}"
                            // sh "mvn package"

                            sh "scp -o StrictHostKeyChecking=no server-config.sh ${DEV_SERVER}:/home/ec2-user"
                            sh "ssh -o StrictHostKeyChecking=no ${DEV_SERVER} 'bash ~/server-config.sh'"
                        }
                    }
                }
        }
        stage('Deploy'){
            agent any
            // input{
            //     message "Select the version of the package"
            //     ok "Version selected "
            //     parameters{
            //         choice(name:'NEWVERSION',choices:['3','4','5'])
            //     }
            // }
            steps{
                script{
                    echo "Deploying the packaged code"
                    // echo "Deploying code version ${params.NEWVERSION}"
                }
            }
        }
    }
}