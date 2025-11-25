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

    stage('Deploy to Azure App Service'){
      steps{
        withCredentials([azureServicePrincipal(credentialsId: env.AZURE_CREDENTIAL_ID,
                                               clientIdVariable: 'AZURE_CLIENT_ID',
                                               clientSecretVariable: 'AZURE_CLIENT_SECRET',
                                               tenantIdVariable: 'AZURE_TENANT_ID')]){

          sh 'ls -l'
          sh 'ls -l public/'
          sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID' 
          
          // Example deployment for index.html in public
          sh 'az webapp deploy --resource-group my-web-app-rg --name my-jenkins-webapp-001 --src-path public/ --target-path .'
          sh 'az logout'
        }
      }
    }
  }
}
