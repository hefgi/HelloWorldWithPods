pipeline {
  agent {
    label 'ios'
  }
  stages {
    stage('Lint') {
      steps {
        sh '''
export LANG=en_US.UTF-8
source ~/.bash_profile
bundle install
pod install
bundle exec fastlane lint
'''
        checkstyle canComputeNew: false, canRunOnFailed: true, defaultEncoding: '', healthy: '2', pattern: 'build/swiftlint.result.xml', unHealthy: '10'
      }
    }
    stage('Unit Tests') {
      steps {
        sh '''
export LANG=en_US.UTF-8
source ~/.bash_profile
pod install
bundle exec fastlane tests
'''
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'xcov_output', reportFiles: 'index.html', reportName: 'Coverage Report', reportTitles: ''])
      }
    }
  }
  // https://github.com/jenkinsci/pipeline-model-definition-plugin/wiki/Reporting-test-results-and-storing-artifacts
  post {
    always {
      //archive "build/**/*"
      archiveArtifacts allowEmptyArchive: true, artifacts: 'NewsReader.ipa,NewsReader.app.dSYM.zip,build/Build/Products/Debug-iphonesimulator/NewsReader.app'
      junit 'build/report.junit'
    }
  }
}