node('maven') {
  stage('Build') {
    git url: "https://github.com/minggyu123/mingyu123-openshift.git"
    sh "mvn package"
  }
  stage('Test') {
    parallel(
      "Httpd Tests": {
        sh "mvn verify -P httpd-test"
      },
      "Discount Tests": {
        sh "mvn verify -P discount-tests"
      }
    )
  }
  stage('Build Image') {
    sh "oc start-build httpd --from-file=target/httpd.jar --follow"
  }
  stage('Deploy') {
    openshiftDeploy depCfg: 'httpd'
    openshiftVerifyDeployment depCfg: 'httpd', replicaCount: 1, verifyReplicaCount: true
  }
}
