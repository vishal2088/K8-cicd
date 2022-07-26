node('master'){
  notify("Started!!!!!")
  currentBuild.displayName = "#1.0." + env.BUILD_ID

  def Namespace
  def ImageName

  try{
    stage ('Environment Varaibles'){
       def userInput = input(
          id: 'userInput', message: 'Let\'s promote?', parameters: [
            choice(name: 'Namespace', choices: 'Dev\nQA\nProd',  description: 'Select Preferred Namespace'),
          ])

      Namespace = "${userInput}"

    }
    stage('Checkout'){
      git 'https://github.com/M-Ayman/wordpress.git'
      sh "git rev-parse HEAD > .git/commit-id"
      imageTag= readFile('.git/commit-id').trim()

    }
    stage('Docker Build, Push'){
      withDockerRegistry([credentialsId: '55d22be4-cff4-4609-a97d-a74ad61ad12b', url: 'https://index.docker.io/v1/']) {
        ImageName = "username/wordpress"
        sh "docker build -t ${ImageName}:${imageTag} ."
        sh "docker push ${ImageName}:${imageTag}"
        sh "docker rmi ${ImageName}:${imageTag}"
          }

      }

    node('ansible'){
      stage('Deploy Wordpress Website'){
        sh "ansible-playbook /home/jenkins/roles/wordpress-deploy/deploy.yml --user=ansible  --extra-vars ImageName=${ImageName} --extra-vars imageTag=${imageTag} --extra-vars Namespace=${Namespace}"

      }
    }



    } catch (err) {
        currentBuild.result = 'FAILURE'
      }
}
