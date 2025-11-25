pipeline {
  
  agent any
  
  environment {
    
    // ID set for your Azure Service Principal credential
    AZURE_CREDENTIAL_ID = 'AzureServicePrincipal'

    // Define Azure Deployment targets
    RESOURCE_GROUP = 'my-web-app-rg'
    WEBAPP_NAME = 'my-jenkins-webapp-001'
  }

  stages {
    
    stage('Checkout'){
      steps{
        // SCM Details
        git url: 'https://github.com/SK-Khatri/building-a-multibranch-pipeline-project.git', branch: 'master'
      }
    }

    stage('Build'){
      steps{
        echo "Hello World!"
      }
    }

    stage('Zip Artifacts') {
            steps {
                sh '''
                    cd public
                    zip -r ../webapp.zip . -x "*.git*" "node_modules/*" "*.md"
                    cd ..
                '''
            }
        }

    stage('Deploy to Azure App Service'){
      steps {
          withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIAL_ID, 
                                                subscriptionIdVariable: 'SUBSCRIPTION_ID',
                                                clientIdVariable: 'CLIENT_ID', 
                                                clientSecretVariable: 'CLIENT_SECRET', 
                                                tenantIdVariable: 'TENANT_ID')]) {
              sh '''
                  az login --service-principal -u $CLIENT_ID -p $CLIENT_SECRET --tenant $TENANT_ID
                  az webapp deployment source config-zip \
                      --resource-group $RESOURCE_GROUP \
                      --name $WEBAPP_NAME \
                      --src webapp.zip
             '''
         }
      }
    }
  }
}
