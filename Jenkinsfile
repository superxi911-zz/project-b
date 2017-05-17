node {
  echo "test ENV"
  echo "BRANCH_NAME'${env.BRANCH_NAME}'"
  echo "CHANGE_ID'${env.CHANGE_ID}'"
  echo "CHANGE_URL'${env.CHANGE_URL}'"
  echo "CHANGE_TITLE'${env.CHANGE_TITLE}'"
  echo "CHANGE_AUTHOR'${env.CHANGE_AUTHOR}'"
  echo "CHANGE_AUTHOR_DISPLAY_NAME'${env.CHANGE_AUTHOR_DISPLAY_NAME}'"
  echo "CHANGE_AUTHOR_EMAIL'${env.CHANGE_AUTHOR_EMAIL}'"
  echo "CHANGE_TARGET'${env.CHANGE_TARGET}'"
  echo "BUILD_NUMBER'${env.BUILD_NUMBER}'"
  echo "BUILD_ID'${env.BUILD_ID}'"
  echo "BUILD_DISPLAY_NAME'${env.BUILD_DISPLAY_NAME}'"
  echo "JOB_NAME'${env.JOB_NAME}'"
  echo "JOB_BASE_NAME'${env.JOB_BASE_NAME}'"
  echo "BUILD_TAG'${env.BUILD_TAG}'"
  echo "EXECUTOR_NUMBER'${env.EXECUTOR_NUMBER}'"
  echo "NODE_NAME'${env.NODE_NAME}'"
  echo "WORKSPACE'${env.WORKSPACE}'"
  echo "JENKINS_HOME'${env.JENKINS_HOME}'"
  echo "JENKINS_URL'${env.JENKINS_URL}'"
  echo "BUILD_URL'${env.BUILD_URL}'"
  echo "JOB_URL'${env.JOB_URL}'"
  
  echo "displayName'${currentBuild.displayName}'"
  echo "description'${currentBuild.description}'"
  
  stage('Checkout') {
        echo "check out"   
        checkout scm
  }
  
  stage('integration') {
    if (params.integration) {
      echo "do integration"
    } else {
      echo "skip integration"
    }
  }

  stage('publish') {
    if (params.publish) {
      echo "do publish"
      echo "docker build image tag: " + params.imageTag
      if (params.autoGitTag) {
        echo "auto git tag: " + params.imageTag
        withCredentials ([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'superxi911', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD']]){
          sh("git tag $imageTag")
          sh("git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/superxi911/project-b $imageTag")
        }
      }
    } else {
      echo "skip publish"
    }
  }
  
  stage('deploy') {
    if (params.deploy) {
      echo "do deploy"
      echo "deploy to " + params.deployTarget
      if (params.deployTarget == "stage") {
        withCredentials([[$class: 'FileBinding', credentialsId: 'cluster57', variable: 'SECRET_FILE']]) {
          sh 'kubectl --kubeconfig=$SECRET_FILE get pod'
        }
      }
    } else {
      echo "skip deploy"
    }
  }
}
