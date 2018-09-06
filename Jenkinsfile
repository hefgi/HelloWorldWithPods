pipeline {
  agent {
    label 'ios'
  }
  stages {
    stage('Init') {
      steps {
        sh'''
export LANG=en_US.UTF-8
source ~/.bash_profile
'''
      }
    }
    stage('Download') {
      steps {
        sh'''
bundle install
pod install
'''
      }
    }
    stage('Lint') {
      steps {
        sh '''
bundle exec fastlane lint
'''
        checkstyle canComputeNew: false, canRunOnFailed: true, defaultEncoding: '', healthy: '2', pattern: 'build/swiftlint.result.xml', unHealthy: '10'
      }
    }
    stage('Unit Tests') {
      steps {
        sh '''
bundle exec fastlane tests
'''
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'xcov_output', reportFiles: 'index.html', reportName: 'Coverage Report', reportTitles: ''])
      }
    }
    stage('Beta Deploy') {
      steps {
        sh '''
bundle exec fastlane beta
'''
      }
    }
  }
  // https://github.com/jenkinsci/pipeline-model-definition-plugin/wiki/Reporting-test-results-and-storing-artifacts
  post {
  */
    always {
      //archive "build/**/*"
      archiveArtifacts allowEmptyArchive: true, artifacts: 'NewsReader.ipa,NewsReader.app.dSYM.zip,build/Build/Products/Debug-iphonesimulator/NewsReader.app'
      junit 'build/report.junit'
    }
    /*
  }
}