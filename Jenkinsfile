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
       post {
          success {
            archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
 }

 }

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
          sh "wget http://himagiri0271.mylabserver.com/rectangles/all/rectnagle_${env.BUILD_NUMBER}.jar" 
          sh "java -jar rectnagle_${env.BUILD_NUMBER}.jar 3 4 "
      } 
      }
     }
}
