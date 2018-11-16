pipeline {
  agent any
  stage('Greenlight Example') {
    steps {
      curl -O https://downloads.veracode.com/securityscan/gl-scanner-java-LATEST.zip
      unzip gl-scanner-java-LATEST.zip gl-scanner-java.jar
      java -jar gl-scanner-java.jar \
        --api_id "${VERACODE_API_ID}" \
        --api_secret_key "${VERACODE_API_SECRET}" \
        --project_name "${env.JOB_NAME}" \
        --project_url "${env.JOB_URL}" \
        --project_ref "${env.GIT_COMMIT}"
    }
  }
  post {
    always {
      archiveArtifacts artifacts: 'results.json', fingerprint: true
    }
  }
}