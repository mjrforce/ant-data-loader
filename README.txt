Install Salesforce Dataloader
This asset uses the .JAR file that is included with the dataloader install. Current zip file has version 40, but this file can be replaced with the most up to date dataloader jar file. Follow these directions to install the dataloader:
https://help.salesforce.com/articleView?id=000239784&type=1

Install ANT Salesforce Migration Tool
https://developer.salesforce.com/docs/atlas.en-us.daas.meta/daas/forcemigrationtool_install.htm

Encrypt Password
Follow steps 1 and 2 of this guide to get your encrypted password value. Make sure to use your password and security token as the password value in step 2.
https://developer.salesforce.com/docs/atlas.en-us.dataLoader.meta/dataLoader/command_line_create_encryption_key.htm

Modify Job Parameters
You can add or update any of the job parameters in the template files. List of parameters found here:
https://developer.salesforce.com/docs/atlas.en-us.dataLoader.meta/dataLoader/loader_params.htm

Generate the Map files
You can generate Map files using the dataloader UI. Start an insert or upsert and generate the map. Dataloader will give you an option to save the map file.

Run the Dataloader
1. Extract Zip file to a root directory
2. Update values in build.properties  
Note: sf.encryptedpassword should be the value from the Encrypt Password section above
3. Open command prompt (cmd)
4. Run “ant -lib salesforce-lib.jar start”

Example Build.XML

<target name="start">
  <gitclone branch="master" folder="Project" uri=""/>
  <sf:deploy username="${sf.username}" password="${sf.password}" serverurl="${sf.endpoint}" deployRoot="./project/src" maxPoll="${sf.maxPoll}" pollWaitMillis="${sf.maxWaitMillis}"/>
  <export file="Account" object="Account" soql="Select Id, Name from Account"/>
  <upsert file="Account" object="Account" externalid="My_External_ID__c" mappingfilepath="./maps/AccountMap.sdl"/>
</target>

<target name="pull">
    <delete dir="./src"/>
    <mkdir dir="./src"/>
    <sf:retrieve username="${sf.username}" password="${sf.password}" serverurl="${sf.endpoint}" retrieveTarget="/src" unpackaged="/package.xml"/>
</target>
