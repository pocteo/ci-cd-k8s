node{
  def Namespace = "default"
  def ImageName = "pocteo/cicd"
  def Creds	= "6243a3a3-e2af-4b2e-a75d-e3b1ac4adb6c"
  try{
    stage('Checkout'){
      git 'https://github.com/pocteo/ci-cd-k8s.git'
      sh "git rev-parse --short HEAD > .git/commit-id"
      imageTag = readFile('.git/commit-id').trim()
    } 
    stage('RUN Unit Tests'){
      sh "npm install"
      sh "npm test"
    }
    stage('Docker Build, Push'){
      withDockerRegistry([credentialsId: "${Creds}", url: 'https://index.docker.io/v1/']) {
        sh "docker build -t ${ImageName}:${imageTag} ."
        sh "docker push ${ImageName}"
      }
    }
    stage('Deploy on K8s'){
      sh "ansible-playbook /var/lib/jenkins/cicd/ansible/deploy.yml  --user=jenkins --extra-vars ImageName=${ImageName} --extra-vars imageTag=${imageTag} --extra-vars Namespace=${Namespace}"
    }
  } catch (err) {
    currentBuild.result = 'FAILURE'
  }
}
