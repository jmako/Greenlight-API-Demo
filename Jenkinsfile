pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
				// bat "echo %JAVA_HOME%"
				// echo "${JAVA_HOME}"
                // bat 'gradle build'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Greenlight Example') {
		    steps {
				withCredentials([usernamePassword(credentialsId: 'edbf3976-7de8-4d18-9e80-30167c96c94e', passwordVariable: 'vkey', usernameVariable: 'vid')]) {

					// don't work:
					// bat "curl -V"  == no command
					// curl -V  == error in file

					iex ((New-Object System.Net.WebClient).DownloadString('https://downloads.veracode.com/securityscan/gl-scanner-java-LATEST.zip'))

					// bat 'curl -O https://downloads.veracode.com/securityscan/gl-scanner-java-LATEST.zip'
					// bat 'unzip gl-scanner-java-LATEST.zip gl-scanner-java.jar'
					// bat """
					//     java -jar gl-scanner-java.jar /
					//         --api_id "${VERACODE_API_ID}" /
					//         --api_secret_key "${VERACODE_API_SECRET}" /
					//         --project_name "${env.JOB_NAME}" /
					//         --project_url "${env.JOB_URL}" /
					//         --project_ref "${env.GIT_COMMIT}"
					// """

					// bat """\
					// 	java -jar C:/Users/Administrator/Desktop/gl-scanner-java-LATEST/gl-scanner-java.jar \
					// 		--api_id "${vid}" \
					// 		--api_secret_key "${vkey}" \
					// 		--project_name "${env.JOB_NAME}" \
					// 		--project_url "${env.JOB_URL}" \
					// 		--project_ref "${env.GIT_COMMIT}" \
					// 	"""
							// --issue_details true \
							// --issue_counts=5:0,4:0,3:0,2:0,1:0,0:0 \
                }
		    }
		}
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
	post {
	    always {
	      archiveArtifacts artifacts: 'results.json', fingerprint: true
	    }
	}
}
