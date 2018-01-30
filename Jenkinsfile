pipeline {
  agent {
    node {
      label 'master'
    }
    
  }
  stages {
    stage('Smoke Deployment') {
      agent {
        node {
          label 'ccpq54'
        }
        
      }
      steps {
        echo 'Smoke Deployment'
        sh '''#!/bin/bash
export JENKINS_NODE_COOKIE=do_not_kill
sh ../deploy_ccp_system_v1.sh ccpcit2'''
      }
    }
    stage('Smoke Test') {
      agent {
        node {
          label 'Windows_FE'
        }
        
      }
      steps {
        echo 'Smoke Test'
        bat(returnStdout: true, script: 'java -jar -Dfilters=-skip -Dtestsuite=ui -Dstory=post_deployment_smoke ..\\endtoend-tests-1.0.0.0-SNAPSHOT-ccp-e2etest.jar')
      }
    }
    stage('Optimums Plan') {
      agent {
        node {
          label 'master'
        }
        
      }
      steps {
        echo 'Optimums Plan Started'
      }
    }
    stage('System Startup') {
      parallel {
        stage('System Startup Thread 1') {
          agent {
            node {
              label 'ccpd8'
            }
            
          }
          environment {
            BUILD_ID = 'dontKillMe'
          }
          steps {
            sh '''#!/bin/bash

shopt -s expand_aliases
source ~/.bashrc
export BUILD_ID=dontKillMe
export JENKINS_NODE_COOKIE=do_not_kill

sm 5
sleep 60
Application_sequence_name="System_Cold_Start_Minimal_Risk_and_Clearing_OPT"
SSI 6 $Application_sequence_name

##System Startup Checker
Integration_env=${172.25.76.91}
sysman_project="lch"
#eEnvironment\'s password
ENV_PASSWORD="ccp123"

Application_sequence_name="System_Cold_Start_Minimal_Risk_and_Clearing_OPT"
# Environment list
CCPD8="ccpd8"
CCPD8_HOST="172.25.76.91"
ENV=(${CCPD8}@${CCPD8_HOST})

export SSHPASS=$ENV_PASSWORD

echo > ~/logs/catalina.out

ERROR_CODE=0
echo "------------------------------------------------------------------------------------------------"
echo "                                    Verify Web URL"
echo "------------------------------------------------------------------------------------------------"
for i in ${ENV[@]}; do
                WEB_URL=`/usr/local/bin/sshpass -e ssh -o \'StrictHostKeyChecking no\' -o \'ConnectTimeout=10\' -nq ${i} \'tail -f ~/logs/catalina.out | grep --line-buffered -m 1 -i "System Ready" | head -1\'`
                if [[ -z $WEB_URL ]] ; then
                                echo "$i --- WEB URL --- Not Accessible"
                                ERROR_CODE=1
                else
                                echo "$i --- WEB URL --- Accessible :-)"
                fi
done
if [[ $ERROR_CODE -eq 1 ]] ; then
                exit 1
fi

for i in ${ENV[@]}; do
                /usr/local/bin/sshpass -e ssh -o \'StrictHostKeyChecking no\' -o \'ConnectTimeout=10\' -nq ${i} \'sh ~/prepare_env_details_file.sh\'
                echo "$i --- Run Scripts --- [DONE!]"
done
unset SSHPASS


echo "System Started"'''
          }
        }
        stage('System Startup Thread 2') {
          steps {
            echo 'System Startup Thread 2'
          }
        }
      }
    }
    stage('E2E Tests') {
      parallel {
        stage('Thread 1') {
          agent {
            node {
              label 'Windows_FE'
            }
            
          }
          environment {
            STORY_LIST = 'MyStory'
          }
          steps {
            bat(script: 'java -jar -Dfilters=-skip -Dstory=%STORY_LIST% ..\\endtoend-tests-1.0.0.0-SNAPSHOT-ccp-e2etest.jar', returnStdout: true)
            archiveArtifacts(fingerprint: true, artifacts: 'target\\jbehave\\view*')
          }
        }
        stage('Thread 2') {
          agent {
            node {
              label 'Windows_FE'
            }
            
          }
          steps {
            sleep 80
          }
        }
        stage('Thread 3') {
          agent {
            node {
              label 'Windows_FE'
            }
            
          }
          steps {
            sleep 60
          }
        }
      }
    }
  }
}