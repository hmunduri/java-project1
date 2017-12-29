pipeline {
  agent none

  stages {
    stage('Unit Tests') {
      agent { 
        label 'apache'
      }

      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
        }     
    }
    stage('build') {

      agent { 
        label 'apache'
      }
      steps {
      sh 'ant -f build.xml -v'

    }
  }
    stage('deploy') {

      agent { 
        label 'apache'
      }
      
	steps {
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"

   }
  }
      stage("Running on CentOS") {
        agent {
          label 'apache'
        }
        steps { 
          sh "wget http://himagiri0271.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar" 
          sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4 "
      } 
      }
       stage("Test on Debian") {
         agent {
           label 'apache'
           docker 'openjdk:8u151-jre'
     }
         steps {
           sh "wget http://http://himagiri0271.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
	   sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4 "
    }
}
}
}
