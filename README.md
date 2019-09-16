## Create a DB diretory

1. Git clone the following

```bash
git@github.com:shanemiller89/json-server-deploy-azure.git
```

2. Copy and paste the content of your database into the db.json file in the newly created directory.

3. Move on to Create the project.

## Create the project

*Make sure you have created an account for azure [here](https://azure.microsoft.com/en-us/free/)*

1 . Create your database (The first steps above)

2 . Install the Azure CLI (Insructions [here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest))

3 . Run this command:

```
az login
```

and follow the login procedure.

4 .Set the deployment username and password:

```bash
az webapp deployment user set --user-name <username> --password <password>
```

5 . Create a resource group for your projects, replace the name to whatever you want just be sure to use the same group name in all commands to come. You only have to create the resource group and service plan once, then you can use the same group and plan for all other apps you create if you like.

```bash
az group create -n NameOfResourceGroup -l northeurope
```

6 . Create a service plan:

```
az appservice plan create -n NameOfServicePlan -g NameOfResourceGroup
```

7 . Create the actual app and supply the service plan and resource group
```bash
az webapp create -n NameOfApp -g NameOfResourceGroup --plan NameOfServicePlan
```

8 . Create deployment details. A git-repo is not created automatically so we have to create it with a command:

```bash
az webapp deployment source config-local-git -n NameOfApp -g NameOfResourceGroup
```

9 . From the command in step 8 you should get a **url** in return. Copy this url and add it as a remote to your local git project, for example:

```bash
git remote add azure <returned url>
```

10 . Now you should be able to push your app:
```bash
git push azure master
```

You should be prompted to supply a password, this should be the password of the deployment user and password you set in step 4. If not, you can choose a different password at your Dashboard for Azure: **[https://portal.azure.com/](https://portal.azure.com/)**

Choose **App Services** in the sidebar to the left and the choose your app in the list that appears then go to **Deployment Center**. Then select **FTP/Credentials**. Then select **User Credentials** and set a username and password. You may have to change the deployment user to the username you just created.<br>
https://docs.microsoft.com/en-us/azure/app-service/app-service-deployment-credentials


Your JSON server should be up and running. The URL of your app is located in your dashboard under App Service, then select your app. Copy and paste the URL and replace with your localhost URL in your fetch calls.

***Note***: If you are using a service like Heroku, make sure your URL starts with https:// not http://. This is a work around and not a true fix. JSON server should only be used for testing purposes on a small scale deployment. Not a full scale deployment.

