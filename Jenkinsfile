pipeline {
  agent {
    kubernetes {
      yamlFile 'KubernetesPod.yaml'
    }
  }
  stages {

    stage('prepare') {
      steps {
        container('maven') {
          dir('project') {
            echo 'check out the project'
            checkout([
              $class: 'GitSCM', 
              branches: [[name: '*/main']], 
              extensions: [], 
              userRemoteConfigs: [[url: 'https://github.com/rsmaxwell/example-java']]
            ])

            echo 'prepare the application'
            sh('./scripts/prepare.sh')

            echo 'build the application'
            sh('./scripts/build.sh')

            echo 'packaging the application'
            sh('./scripts/package.sh')
          }
        }
      }
    }

    stage('test') {
      steps {
        container('tools') {
          dir('project') {
            echo 'testing the application'
            sh('./scripts/test.sh')
          }
        }
      }
    }

    stage('deploy') {
      steps {
        container('maven') {
          dir('project') {
            echo 'deploying the application'
            sh('./scripts/deploy.sh')
          }
        }
      }
    }
  }
}
