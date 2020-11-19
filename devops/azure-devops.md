# deployment/hosting wordpress on Azure

## Getting started

### Required

1. You already have an account on portal.azure.com (to manage infrastructure)
2. You have already workspace AzureDevops for your project (to manage code of your project)
3. download wp core trunk package [download link for wp package](https://core.trac.wordpress.org/browser?order=name)
   and upload it to your github account

### Start

####

1. Go to Devops (https://dev.azure.com/kata-training/) and create a project
2. Set it to private, and import the previous git repo (you may need to authenticate if the repo is private)
3. Create a new ressource on portal.azure.com

   - choose a Web App , call it `kata-training-demo`
   - Choose tier `F1` for data usage
   - click on `Verify and create`
   - then `Create`

4. After deployment is done,
5. Create an `Azure server database for MySQL`
   - Go to `Create resource` from Dashboard, from the marketplace, select `Azure server database for MySQL`
   - Fill the forms, save credentials for admin user (kata/Ruedulouvre25)
   - Deploy the database
   - When deployed, go to Security connection and disable the SSL connection
   - Add `client ip` to be able to access from MySQL workbench
6. Associate a new database resource to your webapp
   - Select the resource `kata-training-demo`
   - Add a new connection string
     - Give it following values :
       - name : `defautlConnection`
       - value : `Database=wordpress;Data Source= kata-db-training.mysql.database.azure.com;User Id=kata;Password=Ruedulouvre25`

- Download and install MySQL workbench :

  - Connect with below credentials :
    - Hostname : `kata-db-training.mysql.database.azure.com`
    - Username/password: `kata@kata-db-training/Ruedulouvre25`
  - Create a new database called `wordpress`
    - On Portal azur, on your db resource, you should have in `available resources` section below, you should have `wordpress`
  - Configure wp-config, go to your workspace azure devops and edit online the `wp-config-sample` to have your wp database credentials

    `wp-config-sample.php`

    ```php
    // ** MySQL settings - You can get this info from your web host ** //
    /** The name of the database for WordPress */
    define( 'DB_NAME', 'wordpress' );

    /** MySQL database username */
    define( 'DB_USER', 'kata@kata-db-training' );

    /** MySQL database password */
    define( 'DB_PASSWORD', 'Ruedulouvre25' );

    /** MySQL hostname */
    define( 'DB_HOST', 'kata-db-training.mysql.database.azure.com' );
    ```

7. Setup of build pipeline

- `Build pipeline` will be executed everytime your code changes and will output an artifact that will be used in the `Release pipeline`
  - Go to [Azure Devops pipelines](https://dev.azure.com/kata-training/kata-formation/_build)
  - Click `Create a new pipeline`
  - Select your existing repo
  - On `Configure your pipeline`, select `Starter pipeline`
  - Copy/paste below YAML file

```yaml
trigger:
  - main

pool:
  name: Hosted Ubuntu 1604
  demands: node.js

steps:
  - task: NodeTool@0
    displayName: "Run a one-line script"
    inputs:
      versionSpec: 10.13.0

  - bash: |
      npm install npm -g
      npm install grunt-cli -g
      npm install
    displayName: "Install Grunt globally"

  - task: Grunt@0
    displayName: "Build Wordpress with Grunt"
    inputs:
      gruntFile: Gruntfile.js

  - task: ArchiveFiles@2
    displayName: "Archive build files"
    inputs:
      rootFolderOrFile: "$(Build.Repository.LocalPath)/build"
      includeRootFolder: false

  - task: PublishBuildArtifacts@1
    displayName: "Publish Artifact: drop"
```

- Click `Save and run` (Build should take 2-3min)

8. Create a `Release Pipeline`
   - This pipeline will execute everytime a new build will be available, then redeploy your site with latest version
   - Choose `Web Deploy` method and click `Save`
   - Go back to `pipeline` tab, click on `Add artifact` and select the source build `kata-formation`
   - On Stage 1, click on `Deploy`
   - Last operation should take approx 3min
   - when Release pipeline is finished : go to https://kata-training-demo.azurewebsites.net
   - configure your admin interface, store somewhere
   ```
     URL admin : https://kata-training-demo.azurewebsites.net/wp-admin/
     id : kata
     mdp : bOd0lb!l$y&QboOMpr
   ```

- Voila now you have access to your wordpress dashboard and wp site is online.
- To see the CICD working, go your AzureDevops repot, edit any file and save it. A build will be triggered and when artifact is published, the release pipeline will execute to redeploy your webapp with latests build.

## further links/ ressources

[Connect Domain Name With Azure Hosting Using NameServer | Domain Link with Azure | DNS Records](https://www.youtube.com/watch?v=k7ZHNeCQS9U&list=PLD1SO-LfilpbHeXav5_ZK84XTgKiPKLQ9&index=7)

[Transfer domain and migrate database](https://www.youtube.com/watch?v=tldaroMmWfc)

[main video youtube helped to do this tutorial](https://www.youtube.com/watch?v=G2e9FHqSS5g)

## credentials

- azure portal admin

admin username : azureuser

pwd: kata2575001

### errors encountered

- infra issues :heavy_exclamation_mark:

```
`France-centre` as infra org

error : Failed to configure continuous delivery on the virtual machine.
Error: Failed to create organization:
Error: TFS.WebApi.Exception: {
"\$id":"1",
"innerException":null,
"message":"Region: SCUS is not available right now,
please try again later or pick a different region",
"typeName":"Microsoft.VisualStudio.Services.HostAcquisition.RegionNotAvailableException, Microsoft.VisualStudio.Services.WebApi",
"typeKey":"RegionNotAvailableException","errorCode":0,"eventId":3000}

`US-Centre` as infra org

Failed to configure continuous delivery. Error: Configuration failed at step: 'Creating assets'. More details: {
"code": "VMExtensionHandlerNonTransientError",
"message": "The handler for VM extension type 'Microsoft.VisualStudio.Services.TeamServicesAgentLinux'
has reported terminal failure for VM extension 'TeamServicesAgentExtension' with error message:
'[ExtensionOperationError] Non-zero exit code: 1,
/var/lib/waagent/Microsoft.VisualStudio.Services.TeamServicesAgentLinux-1.21.0.0/AzureRM.py\n[stdout]\n\n\n[stderr]\nTraceback (most recent call
```

:white_check_mark: Solution : Had to change the hosting region (North America worked)
