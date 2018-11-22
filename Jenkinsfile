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

					// powershell "((New-Object System.Net.WebClient).DownloadString('https://downloads.veracode.com/securityscan/gl-scanner-java-LATEST.zip'))"
					// powershell "(New-Object Net.WebClient).DownloadFile('https://downloads.veracode.com/securityscan/gl-scanner-java-LATEST.zip','C:/Program Files (x86)/.jenkins/jobs/Greenlight_API_pipeline/workspace/gl-scanner-java-LATEST.zip');(new-object -com shell.application).namespace('C:/Program Files (x86)/.jenkins/jobs/Greenlight_API_pipeline/').CopyHere((new-object -com shell.application).namespace('C:/Program Files (x86)/.jenkins/jobs/Greenlight_API_pipeline/gl-scanner-java-LATEST.zip').Items(),16)"
					// powershell '''(New-Object Net.WebClient).DownloadFile(\'https://downloads.veracode.com/securityscan/gl-scanner-java-LATEST.zip\',\'C:/Program Files (x86)/.jenkins/jobs/Greenlight_API_pipeline/workspace/gl-scanner-java-LATEST.zip\');
					// 	Add-Type -AssemblyName System.IO.Compression.FileSystem
					// 	[System.IO.Compression.ZipFile]::ExtractToDirectory(\'C:/Program Files (x86)/.jenkins/jobs/Greenlight_API_pipeline/workspace/gl-scanner-java-LATEST.zip\', \'C:/Program Files (x86)/.jenkins/jobs/Greenlight_API_pipeline/workspace/\')
					// '''

					powershell '''
						$dir = \'C:/Program Files (x86)/.jenkins/jobs/Greenlight_API_pipeline/workspace/\'
						$zipupload = $dir + \'gl-scanner-java-LATEST.zip\';
						(New-Object Net.WebClient).DownloadFile(\'https://downloads.veracode.com/securityscan/gl-scanner-java-LATEST.zip\',$zipupload);

						function Unzip($zipfile, $outdir)
						{
							Add-Type -AssemblyName System.IO.Compression.FileSystem
							$archive = [System.IO.Compression.ZipFile]::OpenRead($zipfile)
							foreach ($entry in $archive.Entries)
							{
								$entryTargetFilePath = [System.IO.Path]::Combine($outdir, $entry.FullName)
								$entryDir = [System.IO.Path]::GetDirectoryName($entryTargetFilePath)

								#Ensure the directory of the archive entry exists
								if(!(Test-Path $entryDir )){
									New-Item -ItemType Directory -Path $entryDir | Out-Null 
								}

								#If the entry is not a directory entry, then extract entry
								if(!$entryTargetFilePath.EndsWith("\\")){
									[System.IO.Compression.ZipFileExtensions]::ExtractToFile($entry, $entryTargetFilePath, $true);
								}
							}
						}

						Unzip -zipfile "$zipupload" -outdir "$dir"
					'''




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
