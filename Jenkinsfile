pipeline {
  agent any
  stages {
    stage('检出') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: env.GIT_BUILD_REF]],
        userRemoteConfigs: [[url: env.GIT_REPO_URL, credentialsId: env.CREDENTIALS_ID]]])
      }
    }
    stage('编译') {
      steps {
        sh './mvnw package -Dmaven.test.skip=true'
      }
    }
    stage('Junit-单元测试') {
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          codingHtmlReport(name: 'my-report', path: 'target/surefire-reports/*.xml', entryFile: 'index.html')

        }

      }
      steps {
        sh './mvnw test'
      }
    }
    stage('打包镜像') {
      steps {
        sh "docker build -t ${ARTIFACT_IMAGE}:${env.GIT_BUILD_REF} ."
        sh "docker tag ${ARTIFACT_IMAGE}:${env.GIT_BUILD_REF} ${ARTIFACT_IMAGE}:latest"
      }
    }
    stage('推送到制品库') {
      steps {
        script {
          docker.withRegistry("${CCI_CURRENT_WEB_PROTOCOL}://${ARTIFACT_BASE}", "${env.DOCKER_REGISTRY_CREDENTIALS_ID}") {
            docker.image("${ARTIFACT_IMAGE}:${env.GIT_BUILD_REF}").push()
            docker.image("${ARTIFACT_IMAGE}:latest").push()
          }
        }

      }
    }
  }
  environment {
    ARTIFACT_BASE = "${CCI_CURRENT_TEAM}-docker.pkg.${CCI_CURRENT_DOMAIN}"
    ARTIFACT_IMAGE = "${ARTIFACT_BASE}/${PROJECT_NAME}/${DEPOT_NAME}/${DEPOT_NAME}"
  }
}