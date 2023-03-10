pipeline{
    agent any
    tools {
        maven "some name"
        jdk "java ledugma"
    }
    
    stages{
        stage("checkout"){
            steps{
                echo "========checking out (loking hella fine)========"
                deleteDir()
                checkout scm
                sh "mvn clean"
                sh "git checkout ${GIT_BRANCH}"
            }
        }
        stage("Calculate and set 3-number version"){
             when {
                expression { GIT_BRANCH.contains('release/') }
            }
            steps{
                echo "========Calculate and set 3-number version========"
                script{
                    sh """
                    
                    NEXTVERSION=\$(git describe --tags | cut -d '-' -f1 | awk -F. -v OFS=. '{\$NF += 1 ; print}')
                    
                    if [ \$NEXTVERSION -eq  "" ] 
                    then
                        NEXTVERSION=\$(git describe --tags | cut -d '-' -f1 | awk -F. -v OFS=. '{\$NF += 1 ; print}')
                        echo "\${NEXTVERSION}" > v.txt
                    else
                        echo "\$(git branch | grep '*'| cut -d '/' -f2).1" > v.txt
                    fi
                    """
                    sh "mvn versions:set -DnewVersion=\${NEXTVERSION}"                
                }
            }
            post{
                always{
                    echo "========Calculate and set 3-number version finished========"

                }
                success{
                    echo "========Calculate and set 3-number version executed successfully========"
                }
                failure{
                    echo "========Calculate and set 3-number version execution failed========"
                }
            }
        }
        
        stage("build"){
            steps{
                echo "========executing build========"
                withMaven {
                    sh "mvn verify"
                }                
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========A executed successfully========"
                }
                failure{
                    echo "========A execution failed========"
                }
            }
        }
        stage("publish "){
            when {
                expression { GIT_BRANCH.contains('release/') || BRANCH_NAME == 'main' }
            }
            steps{
                echo "========executing publishing to artifactory========"
                withMaven {
                    configFileProvider([configFile(fileId: '0a5edd42-4379-4509-a49e-d8ba1384edeb', variable: 'set')]) {
                        sh "mvn -s ${set} deploy"
                    } 
                } 
            }
            post{
                always{
                    echo "========always daaaaa========"
                }
                success{
                    echo "========A executed successfully========"
                }
                failure{
                    echo "========A execution failed========"
                }
            }
        }
        stage("push tag"){
            when {
                expression { GIT_BRANCH.contains('release/') }
            }
            steps{
                echo "========executing pushing tag========"
                
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========A executed successfully========"
                }
                failure{
                    echo "========A execution failed========"
                }
            }
        }
        stage("C"){
            steps{
                echo "========executing A========"
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========A executed successfully========"
                }
                failure{
                    echo "========A execution failed========"
                }
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}