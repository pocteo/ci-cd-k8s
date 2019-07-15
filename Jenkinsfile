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
  } catch (err) {
    currentBuild.result = 'FAILURE'
  }
}
