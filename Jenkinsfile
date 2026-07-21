// GitHub push -> webhook -> Jenkins -> SSH/rsync -> Apache servers (/var/www/html).
// Uses the SSH Agent plugin + the 'webservers-ssh-key' credential. Put at repo ROOT.

pipeline {

  agent any

  environment {

    AZ_ACCOUNT = '<youruniquestorage>'   // same as $STG

    AZ_SHARE   = 'webcontent'

  }

  stages {

    stage('Checkout') { steps { checkout scm } }

    stage('Deploy to ACI (file share)') {

      steps {

        withCredentials([string(credentialsId: 'azure-storage-key', variable: 'AZ_KEY')]) {

          sh '''

            az storage file upload-batch \

              --account-name "$AZ_ACCOUNT" --account-key "$AZ_KEY" \

              --destination "$AZ_SHARE" --source . \

              --pattern "*.html" --no-progress

          '''

        }

      }

    }

  }

}
