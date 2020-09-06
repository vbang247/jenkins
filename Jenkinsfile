pipeline {
 agent {
       // ec2 cloud: 'jenkins-floating-slave', template: 'jenkins-floating-slave'
       node {
           label 'jenkins-floating-slave'
       }
 }
 parameters {
  string(name: 'application_url', defaultValue: 'http://localhost:80', description: 'url for zap attack')
 }
 stages {
  stage('Setup') {
    when {
                // Only run the vulnerable app container if no other URL mentioned
                expression { params.application_url == 'http://localhost:80' }
            }
   steps {

    sh "sudo  sudo docker run --rm --name vulnerable_app -d -p 80:80 vulnerables/web-dvwa"

   }
  }
  stage('Test') {

   steps {
    dir('/home/ec2-user/ZAP_2.9.0') {
     sh "./zap.sh -quickurl ${application_url} -quickprogress -quickout ${WORKSPACE}/report.xml -cmd"
    }
   }
  }

  stage('Cleanup') {
        when {
                // Only run this step if no other URL mentioned
                expression { params.application_url == 'http://localhost:80' }
            }
   steps {
    script {
     sh "sudo docker stop vulnerable_app"
    }
   }
  }


 }
 post {
  failure {
   echo "Pipeline currentResult: ${currentBuild.currentResult}"
  }

  success {
      echo "Pipeline currentResult: ${currentBuild.currentResult}"
      archiveArtifacts artifacts: 'report.xml', fingerprint: true
   publishHTML(target: [
    allowMissing: false,
    alwaysLinkToLastBuild: false,
    reportDir: '',
    keepAll: true,
    reportFiles: 'report.xml',
    reportName: "ZAP Report"
   ])

  }
 }
}
