pipeline {
  agent none
  
  stages {
    stage('Unit Tests'){
      agent {
        label 'apache'
      }
      steps {
	sh 'ant -f test.xml -v'
	junit 'reports/result.xml'
      }
    }
    stage ('build'){
      agent {
        label 'apache'
      }
      steps {
        sh 'ant -f build.xml -v'
        }
      post {
        success {
          archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
      }

    }
    stage ('deploy') {
      agent {
        label 'apache'
      }
      steps {
        sh "cp dist/rectangle_${env.BUILD.NUMBER}.jar /var/www/html/rectangles/all/"
      }
    }
    stage ("Running on CentOS") {
    agent {
        label 'CentOS'
      }
      steps {
        "wget http://nikolalecic5c.mylabserver.com/rectangles/all/rectangle_${env.BUILD.NUMBER}.jar"
        sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
      }
    }
    stage("Test on Debian") {
      agent {
        docker 'openjdk:8u121-jre'
      }
      steps {
        sh "wget http://nikolalecic5c.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
      }
    }
  }  
}
