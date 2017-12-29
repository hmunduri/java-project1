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
        sh "mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}"
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"

   }
  }
      stage("Running on CentOS") {
        agent {
          label 'apache'
        }
        steps { 
          sh "wget http://himagiri0271.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar" 
          sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4 "
      } 
      }
       stage("Test on Debian") {
         agent {
           docker 'openjdk'
     }
         steps {
           sh "wget http://himagiri0271.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
	   sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4 "
    }
}
       stage('Promote to Green') {
	agent {
          label 'apache' 
         }
        when { 
           branch 'master'
       }
	steps{
           sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
      	}
	}	 	
       stage('Promote Development Branch to Master'){
        agent {
    
          label 'apache'
        }
	when {
	  branch 'development'
	 }
  	steps {
	    echo "Stasing any local changes"
            sh "git stash"
            echo "Checking out development Branch"
	    sh 'git checkout development'
            echo 'Checking out Mster branch'
            sh 'git checkout master'
            echo 'Merging Development into Master Brabch'
            sh 'git merge development'
	    echo 'Pushing to Origin Master'
            sh 'git push origin master'
	}



}
}
}
