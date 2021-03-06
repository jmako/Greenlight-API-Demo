pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                bat 'gradlew build'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Greenlight Scan') {
		    steps {
				withCredentials([usernamePassword(credentialsId: '30925d33-3b62-4d2c-85ae-45de4e9df498', passwordVariable: 'vkey', usernameVariable: 'vid')]) {

					powershell '''
						$dir = (Get-Item -Path ".\\").FullName
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

					bat """\
						java -jar ./gl-scanner-java.jar \
							--api_id "${vid}" \
							--api_secret_key "${vkey}" \
							--project_name "${env.JOB_NAME}" \
							--project_url "${env.JOB_URL}" \
							--project_ref "${env.GIT_COMMIT}" \
							--issue_details false \
							--issue_counts=5:2,4:2,3:7,2:5,1:5,0:5 \
					"""
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
