pipeline{
agent any
tools{
maven 'maven'
}
stages{
stage('checkout the code'){
steps{
 
git url:'https://github.com/naimishdev/springboot-maven-course-micro-svc.git', branch: 'master'
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
       agent any
       steps {
        sh 'docker build -t -f Dockerfile naimishdev/spring-petclinic:latest .'
      }
    }

}
}
