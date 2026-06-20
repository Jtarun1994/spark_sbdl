pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
               bat 'python -m venv venv'
               bat 'venv\\Scripts\\pip install --upgrade pip'
               bat 'venv\\Scripts\\pip install pyspark==4.0.3 pytest chispa faker nutter'
            }
        }
        stage('Test') {
            steps {
               bat 'venv\\Scripts\\pytest -v'
            }
        }
        stage('Package') {
	    when{
		    anyOf{ branch "master" ; branch 'release' }
	    }
            steps {
               bat 'powershell Compress-Archive -Path lib -DestinationPath sbdl.zip -Force'
            }
        }
		stage('Archive Artifacts') {
            when {
                anyOf { branch 'master'; branch 'release' }
            }
            steps {
                archiveArtifacts artifacts: 'sbdl.zip, sbdl_main.py, conf/**, log4j.properties, sbdl_submit.sh', fingerprint: true
            }
        }
    }
}
