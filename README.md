# on your server add jenkins

#curl https://releases.rancher.com/install-docker/17.03.sh | sh

#usermod -aG docker jenkins

#yum install python-pip

#sudo pip install ansible

#wget https://storage.googleapis.com/kubernetes-helm/helm-v2.8.1-linux-amd64.tar.gz

#tar -xzvf helm-v2.8.1-linux-amd64.tar.gz

#sudo mv linux-amd64/helm /usr/local/bin

#copy the content of kubectl under ~/.kube/config

#helm init

navigate http://your-ip:8080/pluginManager/available
search for this plug-in "CloudBees Docker Build and Publish"
click Download Now and check the box to restart

how create a pipeline?
1- select New Item from left side
2- enter the name and select the pipeline type
3- copy the content of Jenkinsfile to the pipeline

how to use pipeline syntax?
Navugate http://your-ip:8080/job/job-name/pipeline-syntax/
-> select git and provide the repo URL and user-name/password"if it is a private", it will generate the syntax for you
-> select the withdocker-registry which installed before and provide it with credentials of harbor registry, copy this content inside the block

  ImageName = "harbor-registry.com:port/wordpress"
  sh "docker build -t ${ImageName}:${imageTag} ."
  sh "docker push ${ImageName}:${imageTag}"
  sh "docker rmi ${ImageName}:${imageTag}"


# Replace this line ImageName = "harbor-registry.com:port with your registry and port


# replace the previous content in Jenkinsfile, will look like


stage('Docker Build, Push'){

  withDockerRegistry([credentialsId: '55d22be4-cff4-4609-a97d-a74ad6133a', url: 'https://index.docker.io/v1/']) {

    ImageName = "harbor-registry.com:port/wordpress"

    sh "docker build -t ${ImageName}:${imageTag} ."

    sh "docker push ${ImageName}:${imageTag}"

    sh "docker rmi ${ImageName}:${imageTag}"

      }

  }
