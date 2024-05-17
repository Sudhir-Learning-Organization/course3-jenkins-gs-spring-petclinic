pipeline {
    agent any
    
    stages{
        stage("checkout"){
            steps{
                git branch: 'main', url: 'https://github.com/Sudhir-Learning-Organization/course3-jenkins-gs-spring-petclinic.git'
            }
        }
        stage("build"){
            steps{
                sh "./mvnw package"
            }       
        }
        stage("capture"){
            steps{
                archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
                jacoco()
                junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
            }              
        }
    }

  post{
      regression {
          emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}", recipientProviders: [developers()], subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}", to: 'jenkins@mailbox.com'
      }
  }
}
