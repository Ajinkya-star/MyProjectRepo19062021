pipeline{
    tools{
        maven 'mymaven'
        jdk 'myjava'
    }
    agent none
   stages{
       stage('clonerepo'){
           agent any
           steps{
           git 'https://github.com/Ajinkya-star/DevOpsClassCodes.git'
           }
       }
       stage('compile'){
           agent {label 'linux_slave'}
          steps{
           git 'https://github.com/Ajinkya-star/DevOpsClassCodes.git'
           sh 'mvn compile'
          }
       }
       stage('CodeReview'){
           agent {label 'linux_slave'}
          steps{
           sh 'mvn pmd:pmd'
          }
       }
       stage('UnitTest'){
           agent {label 'win_slave'}
          steps{
           git 'https://github.com/Ajinkya-star/DevOpsClassCodes.git'
           bat 'mvn test'
          }
          post {
               success {
                   junit 'target/surefire-reports/*.xml'
               }
           }	
       }
       
        stage('MetricCheck'){
        agent any
        steps{
                  sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
              }
        post {
        success {
	           cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false                  
               }
           }		
          }
        stage('Package'){
              agent {label 'win_slave'}
              steps{
                  git 'https://github.com/Ajinkya-star/DevOpsClassCodes.git'
                  bat 'mvn package'
              }
          }
          
          
   }
    
}
