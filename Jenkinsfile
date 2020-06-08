pipeline {

//For global variable
/*     environment {
        
      
    } */ 

  agent {
    docker {
      image 'runtime-tooling'
      args '-v ${PWD}:/mosaiq-app -w :/mosaiq-app:ro'
      reuseNode true
      }
  }
  stages {
    stage('QA') {
      parallel {

         stage('Release') {

            steps {

              sh " mkdir -p /tmp/build-release "
              sh " cd /tmp/build-release && cmake -DCMAKE_BUILD_TYPE=Release /var/lib/jenkins/workspace/${env.JOB_NAME} &&  cmake --build ."
              //sh " cp /tmp/build-release/bin/mosaiqruntimeprojectname /var/lib/jenkins/workspace/${env.JOB_NAME}/mosaiqruntimeprojectname-release"
              //sh " cp /tmp/build-release/bin/mosaiqruntimeprojectname-tests /var/lib/jenkins/workspace/${env.JOB_NAME}/mosaiqruntimeprojectname-tests"

          }
          environment {
      
            CONAN_USER_HOME = "/tmp/build-release"
            CONAN_NON_INTERACTIVE = 1
           } 
        } 
       stage('Address Sanitizer') {
          steps {
            
              
              sh " mkdir -p /tmp/build-sanitizer "
              sh " cd /tmp/build-sanitizer && cmake -fsanitize=address /var/lib/jenkins/workspace/${env.JOB_NAME} &&  cmake --build ."
              //sh " cp /tmp/build-sanitizer/bin/mosaiqruntimeprojectname /var/lib/jenkins/workspace/${env.JOB_NAME}/mosaiqruntimeprojectname-asanitizer"
          }

          environment {
      
            CONAN_USER_HOME = "/tmp/build-sanitizer"
            CONAN_NON_INTERACTIVE = 1
           } 
        } 
         

         stage('Memory Sanitizer') {
          steps {
                         
              sh " mkdir -p /tmp/build-mem-sanitizer "
              sh " cd /tmp/build-mem-sanitizer && cmake -fsanitize=memory /var/lib/jenkins/workspace/${env.JOB_NAME} &&  cmake --build ."
          }

          environment {
      
            CONAN_USER_HOME = "/tmp/build-mem-sanitizer"
            CONAN_NON_INTERACTIVE = 1
           } 
        }  


     }
          post {
        failure {
            echo 'This build has failed. See logs for details.'
        }
      }
    }

    stage('DEPLOYMENT'){
      parallel {
        stage('Result') {
          steps {
            sh 'echo Reporting...'
          }
        }
          stage('Deploy Conan Artifacts') {
          steps {
              sh 'echo Deployment part'     
             
            //  sh "conan remote add mosaiq-local http://192.168.1.221:8082/artifactory/api/conan/mosaiq-local"
            //  sh "conan user ${CONAN_USER_NAME} -p ${CONAN_PASSWORD} -r=mosaiq-local" 
            //  sh 'conan upload "" -r=mosaiq-local -c' 
            
          }
        }  
      }
    }
  }
}
