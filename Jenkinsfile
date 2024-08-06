pipeline{
agent any
tools{
maven 'maven'
}
stages{
stage('checkout the code'){
steps{
 
git url:'https://github.com/sanjeev135/springboot-maven-course-micro-svc.git', branch: 'master'
}
}
stage('build the code'){
steps{
sh 'mvn clean package'
}
}
stage("sonar quality check"){
steps{
script{
withSonarQubeEnv(installationName: 'sonarqube', credentialsId: 'sonarqube') {
sh 'mvn sonar:sonar '
}
 
timeout(time: 1, unit: 'HOURS') {
def qg = waitForQualityGate()
if (qg.status != 'OK') {
error "Pipeline aborted due to quality gate failure: ${qg.status}"
}
}
}
}
}
stage('Docker Build') {
       steps {
        sh 'docker build -t sanjeev135/spring-petclinic:latest .'
      }
    }
 
       stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'Sanj9431@', usernameVariable: 'sanjeev135')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push sanjeev135/spring-petclinic:latest'
        }
      }
 
}
}}
