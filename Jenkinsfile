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
          label 'master'
        }
        
      }
      steps {
        echo 'Smoke Deployment'
      }
    }
    stage('Smoke Test') {
      agent {
        node {
          label 'master'
        }
        
      }
      steps {
        echo 'Smoke Test'
      }
    }
    stage('E2E Master') {
      agent {
        node {
          label 'master'
        }
        
      }
      steps {
        echo 'E2E Master'
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
    stage('Thread 1') {
      parallel {
        stage('Thread 1') {
          agent {
            node {
              label 'master'
            }
            
          }
          environment {
            THREAD_1_STORIES = 'MyStory'
          }
          steps {
            echo 'Thread 1 started'
          }
        }
        stage('Thread 2') {
          steps {
            echo 'Thread 2 Started'
          }
        }
        stage('Thread 3') {
          steps {
            echo 'Thread 3 Started'
          }
        }
      }
    }
    stage('System Startup') {
      steps {
        sh '''#!/bin/bash

shopt -s expand_aliases
source ~/.bashrc
export BUILD_ID=dontKillMe 

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
    stage('E2E Test') {
      agent {
        node {
          label 'Windows_FE'
        }
        
      }
      environment {
        STORY_LIST = ''
      }
      steps {
        bat(script: 'java -jar -Dfilters=-skip -Dstory=%STORY_LIST% ..\\endtoend-tests-1.0.0.0-SNAPSHOT-ccp-e2etest.jar', returnStdout: true)
      }
    }
  }
}