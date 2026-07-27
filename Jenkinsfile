pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/0hJieun/ktcloudinfrajenkins.git', branch: 'main'
      }
    }
    stage('docker image build and push to hub') {
      steps {
        sh '''
          echo "###########날짜 태그 생성##############"
          IMAGE_TAG=$(TZ=Aisa/Seoul date _%m%d)
          echo "IMAGE_TAG=${IMAGE_TAG}"

          echo "########## 이미지 빌드################"
          docker build -t jieun3113/ktcloudinfra:${IMAGE_TAG} .




          echo "##########도커 이미지 푸시################"
          docker push jieun3113/ktcloudinfra4:${IMAGE_TAG}
        '''
      }
    }
    stage ('delivery and deployment using k8s') {
      steps {
        sh '''
        ansible master -m copy -a "src=deploy.yml dest=/root/deploy.yml"
        ansible master -m shell -a "kubectl --kubeconfig=/etc/kubernetes/admin.conf apply -f deploy.yml"
        '''
      }
    }
  }
  post {
      success {
          echo 'Pipeline succeeded!'
          // 성공에 따른 스크립트 동작등도 가능하다!!!  결과 코드 : 0
      }
      failure {
          echo 'Pipeline failed!'
      }
  }
}
