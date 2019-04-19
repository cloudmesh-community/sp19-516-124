# Azure VM Management

:o: you must not use the word I

:o: if time allows switch images to light background. HOwever focus on project first

:o: the terminal outputs you have should not be done as images, but pasted and copied ASCII text

| Andrew Garbe
| agarbe@iu.edu
| Indiana University
| hid: sp19-516-124
| github: [:cloud:](https://github.com/cloudmesh-community/sp19-516-124/edit/master/project-report/report.md)

## Abstract

Microsoft Azure is one of the leading cloud computing provides on the market today. 
With government, business and other organizations that have chosen this cloud computing technology, 
it would be unreasonable to think that any large-scale environment would be solely managed through 
the Azure portal GUI interface. 

The solution to effectively managing an Azure environment would occur through integration with technologies 
that make managing the Azure environment scalable and efficient.

## Introduction

The goal of this project is to interface with an Azure Virtual Machine using the Apache Libcloud Python library as well as the Microsoft CLI interface. To accomplish this goal a Micrsoft Azure environment will need to be configured and administrated in order to use the Apache Libcloud Python library and Azure CLI Interaface. This report will take you through the steps to configure the Azure enviroment as well as touching on the Apache Libcloud Python ASM and ARM libraries and Azure CLI Interaface. Last I will outline findings and challenges encountered thought this process.

## Requirements

* A computer where you have administrative rights to install applications.
* Python installed. (Version 3.7.2 at the time of this writing)
* Python IDE such as PyCharm
* Microsoft Azure Student Subscription

### Setup - Student Azure Portal
To start working in the Azure environment, you will to first need to set up an Azure Student Account. 
You can set up and activate an Azure student account at <https://azure.microsoft.com/en-us/free/students/>. 

When you sign up for an Azure free account, you get a Free Trial subscription, which provides you $200 in Azure credits for 30 days and 12 months of free services. At the end of 30 days, Azure disables your subscription. Your subscription is disabled to protect you from accidentally incurring charges for usage beyond the credit and free services included with your subscription. To continue using Azure services, you must upgrade your subscription to a Pay-As-You-Go subscription. After you upgrade, your subscription still has access to free services for 12 months. You only get charged for usage beyond the free services and quantities.

The Azure Student Account requires that you to activate the account after 30 days of use. 
If you do not activate, you will loose access to your Azure Student Account and can not use the services.

The Azure student account FAQ will likely answer questions you might have pertaining to an Azure Student Account, what you will have access to, how long you will enjoy access, and additional general overview information including terms of the account. <https://azure.microsoft.com/en-us/free/free-account-students-faq/>

Once you have set up the Azure Student account, you will gain access the Azure environment through the Azure Portal <https://portal.azure.com>. To log in, please use the credentials you determined during the set up.

### Create a Ubuntu Server 18.04 LTS Virtual Machine in Azure

TBD
In an effort to interact with an Azure Virtual Machine using Apache LibCloud, Azure SDK and/or Azure CLI, I have created a virtual machine for interaction. Here are the steps that I performed to create a Linux Ubuntu Server. 

To start, go to the Azure Portal <https://portal.azure.com>.
Next, the locate the `Virtual Machines` option and select it:

![@label](images/virtualmachines.png)

Then to create a new virtual machine, select `Add`:

![@label](images/addvirtualmachines.png)

This will present you the configuration options needed to create a new virtual machine:

![@label](images/createavirtualmachine.png)

To configure a machine, I chose the following options:
Subscription: “Azure for Students” (default)

Resource Group: `AndrewCloudSevicetest` (Create new one if you do not have an available option.)

Virtual Machine Name: `EfiveothreeTest`

Region: `Central US` (default)

Availability Options: `No infrastructure redundancy required` (default)

Image: `Ubuntu Server 18.04 LTS`

Size: `Standard D2s v3, 2 vcpus, 8GB memory`

Authentication type: `password` (Choose a username and a password that meet the requirements).

The next configuration section is `Disks`:

![@label](images/disks.png)

I chose all the default configurations settings except for `OS disk type`, of which I selected the `Standard SSD` option.

For `Networking` I chose all the default configuration settings:

![@label](images/networking.png)

For `Management` I chose all the default configuration settings:

![@label](images/management.png)

Last, I created the virtual machine:

![@label](images/createvmvalidation.png)

Once the new VM has been created, Naviagate back to the `Virtual machines` and now discover your Virtual Machine:

![@label](images/newvmaftercreation.png)

After creation the virtual machine will be in a `running` status. You will want to decide if you want your virtual machine in a `running` status, else stop the VM so that you do not waste resources. 

### Remote access the Virtual Machine 
To access my virtual machine, I used Putty client< https://www.putty.org/>.

To use Putty and access the virtual machine, I have configured a DNS name in Azure.
This is performed in the Virtual Machine configuration under `DNS name`:

![@label](images/dns.png)

Click `Configure`.
I chose a `static` IP setting so that so that I will not have to look up a dynamic IP:

![@label](images/static.png)

Then click `save`.

Note: If you have not configured the `port` that connection will occur, then connection will not be successful.

In your Virtual machine settings click `Connect` and review the connection settings.
I have designed `port 22` to be the port that will remote connect to the virtual machine:

![@label](images/conncetandport.png)

Next, I launched the Putty client and enter in my DNS name of the virtual machine to connect to the machine:

![@label](images/putty.png)

The first time the environment is accessed Putty, Putty will prompt to cache your servers host key. 
I selected `Yes` when prompted:

![@label](images/cacheputtykey.png)

After the key is cached, next time you enter the Puttry client, you will be prompted to enter your server credentials as specified in the virtual machine setup.

Once credentials are provided, you will be logged into your virtual machine:

![@label](images/loggedinviaputty.png)

 
## Apache Libcloud Azure ASM Compute Driver

TBD

Apache Libcloud is a Python library which hides differences between different cloud provider APIs and allows you to manage different cloud resources through a unified and easy to use API. For additional reference and/or more detail, you can read at <https://libcloud.readthedocs.io/en/latest/index.html>. 
The Azure ASM Compute Driver allows you to integrate with Microsoft Azure Virtual Machines service using the Azure Service Management (ASM) API. This is the “Classic” API, please note that it is incompatible with the newer Azure Resource Management (ARM) API, which is provided by the azure arm driver.

### Connecting to Azure
To connect to Azure, you need your subscription ID and certificate file.

#### Generating and uploading a certificate file and obtaining subscription ID
To be able to connect to the Azure, you need to generate a X.509 certificate which is used to authenticate yourself and upload it to the Azure Management Portal.
On Linux, you can generate the certificate file using the commands shown below:

```bash
$  openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout azure_cert.pem -out azure_cert.pem
$  openssl x509 -inform pem -in azure_cert.pem -outform der -out azure_cert.cer
```

Since I am using Windows 10 to work with the ASM Compute Driver in PyCharm,
I used the WINSCP application to connect to my Linux machine so that I can copy the "azure_cert.pem" certificate that was generated in my Linux `/home` directory to any location of my choosing on my Windows 10 machines file system. An example would be `C:\Users\Andrew.garbe\Downloads\2019 Spring Data Science\Project\azure_cert.pem`.

Certificates are used in Azure for cloud services service certificates and for authenticating with the management API management certificates.
A certificate overview for Azure Cloud Service can be referenced at: <https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-certs-create>.

Once you have an available certificate, you will then need to upload the certificate to Azure. Information about this process can be located at <https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-configure-ssl-certificate-portal>. 
The steps I used are as follows:

 Connect to the Azure Portal and login: <https://portal.azure.com>
 
 Connect to the `All services` option on the left side of the Azure Portal.
 
![@label](images/allservices.png)

Under General section, locate the `Subscriptions` option:

![@label](images/subscriptions.png)

Under subscriptions, select the subscription `Azure for Students`:

![@label](images/azureforstudents.png)

In the `Azure for Students` options select `Management certificates`:

![@label](images/managementcertificates.png)

Next, select `Upload` to upload your certificate: 

![@label](images/upload.png)

Choose a subscription and provide the path to the .Cer Certifcate file, then select `Upload` to associate the certificate with your subscription:

![@label](images/uploadcertificates.png)

Once uploaded, the certificate will show up in the `Azure for Students – Management certificates` section: 

![@label](images/azureforstudentsmanagementcertifcates.png)

Take note of the `subscriptionID` associated with the certificate as this will be need to be referenced when instantiating the Libcloud Azure ASM Compute Driver.

You should now have a certificate associated with an Azure subscription.


Now that you have a certificate you are ready to interact with the Libcloud Azure ASM Compute Driver. To do so, you will need to open a Python IDE. For purposes of this class I am using Pycharm Edu.   

Following the `Azure ASM Compute Driver Documentation` at <https://libcloud.readthedocs.io/en/latest/compute/drivers/azure.html>, once you have generated the certificate file and obtained your subscriptionID, you can instantiate the driver as follows:

```
from libcloud.compute.types import Provider
from libcloud.compute.providers import get_driver

cls = get_driver(Provider.AZURE)
driver = cls(subscription_id='<SubscriptionIDGoesHere>',
             key_file='<Path to a certificate azure_cert.pem file>')
```

One instantiated, the driver is ready for use with the API methods listed in the documentation: <https://libcloud.readthedocs.io/en/latest/compute/drivers/azure.html> 

An example of an Integration that I have had success with is creating an Azure cloud service:

![@label](images/excloundcreateservice.png)

An example of Python code example of creating a Azure cloud service named “e503CloudServicetest” would look like this:

```
#ex_create_cloud_service(name, location, description=None, extended_properties=None)
driver.ex_create_cloud_service('e503CloudServicetest', 'Central US', description=None, extended_properties=None)
```

Once the `ex_create_cloud_service` Python code has been executed, I am now able to the `e503CloudServicetest` cloud service in the Azure Portal under `All resources`:   

![@label](images/e503cloudservicetest.png)



## Apache Libcloud Azure ARM Compute Driver

TBD

Apache Libcloud is a Python library which hides differences between different cloud provider APIs and allows you to manage different cloud resources through a unified and easy to use API. For additional reference and/or more detail, you can read at https://libcloud.readthedocs.io/en/latest/compute/drivers/azure_arm.html. 

The Azure driver allows you to integrate with Microsoft Azure Virtual Machines provider using the Azure Resource Management (ARM) API. Azure Virtual Machine service allows you to launch Windows and Linux virtual servers in many datacenters across the world. To connect to Azure you need your tenant ID and subscription ID.

### Creating a Service Principal
The following directions are based on creating an azure Service Principal. This process can be performed using either Powershell or    through the Azure Portal. 

 * PowerShell Service Principal creation: 
   The following supporting information and steps are used to create an Azure Service Principal using Windows PowerShell:
<https://azure.microsoft.com/en-us/documentation/articles/resource-group-authenticate-service-principal/>. 

 * Azure Portal Service Principal creation: 
   The following supporting information and steps are used to create an Azure Service Principal using the Azure Portal:
<https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal>.

Note: This process assumes that you have an active Azure Student Subscription.

The following steps show you how to create a new Azure Active Directory application and service principal that can be used with the role-based access control. 
When you have code that needs to access or modify resources, you can create an identity for the app. This identity is known as a service principal. 
You can then assign the required permissions to the service principal. 

The follwoing steps will show you how to use the Azure Portal to create the service principal. 
The steps outline a single-tenant application where the application is intended to run within only one organization. 
You typically use single-tenant applications for line-of-business applications that run within your organization.
This is modifiyed to suit my project and has been derived from the following steps: <https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal>

First connect to the Azure Portal and login: <https://portal.azure.com>
Then select the Azure `Active Directory` option.
Choose `App Registrations` in the under the `Manage` section:

![@label](images/appregistrations.png)

Next, select the `New application registration` option:

![@label](images/newappregistration.png)

Provide a name and URL for the application. I made up a fake URL for my app. 

Next, select `Web app / API` for the type of application you want to create. 
Note: You cannot create credentials for a Native application. 
To read more about this you can access the following supporting information: <https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/application-proxy-configure-native-client-application>.

After setting the values, select `Create` to create your Azure AD application and service principal.

![@label](images/createapp.png)

The result of the app creation should resemble something close to the following:

![@label](images/e503testapp.png)

### Assign the application to a role
To access resources in your subscription, you must assign the application to a role.
You can set the scope at the level of the subscription, resource group, or resource. Permissions are inherited to lower levels of scope. For example, adding an application to the Reader role for a resource group means it can read the resource group and any resources it contains.

Next select the `All services` option.
Under the `General` section chose `Subscriptions`:

![@label](images/subscriptions_two.png)

Chose the Subscription to assign the Application ID to. For example, my Subscrition is `Azure for Students`: 

![@label](images/subscriptiondetail.png)

If you do not see the Subscription  that you are looking for, select `global subscriptions` filter. 
Make sure the subscription you want is selected for the portal.

Select the `Access control (IAM)` option. 

![@label](images/accesscontroliam.png)

Choose the `Add a Role Assignment option`:

![@label](images/addroleassignment.png)

To allow the application to execute actions like `reboot`, `start` and `stop` instances, select the `Contributor` role. 

By default, Azure AD applications are not displayed in the available options. To find your application, search for the name and select it. 

Select `Save` to finish assigning the role. 
You see your application in the list of users assigned to a role for that scope.

![@label](images/contributer.png)

A service principal is now set up.

### Get values for signing in (Get Tenant ID)
When programmatically signing in, you need to pass the tenant ID with your authentication request.

Connect to the Azure Portal and login: <https://portal.azure.com>
Select the `Azure Active Directory Default Direcctory overview` select the `Properties` option.

![@label](images/defaultdirectoryproperties.png)

Copy the `Directory ID` to get your `Tenant ID`.

![@label](images/directoryid.png)

### Get an Application ID and Authentication Key
You also need the ID for your application and an authentication key. 
To get those values, use the following steps:

Select App registrations in Azure AD, select your application:

![@label](images/andrewtestapp.png)

Copy the `Application ID`so that you can store it in your application code.
Do this by choosing the application `Settings` and locating `Keys`, then selecting `Keys`.

![@label](images/applicationsettingskey.png)

Provide a description of the key, and a duration for the key. When done, select `Save`.
After saving the key, the value of the key is displayed. 

Copy this value because you are not able to retrieve the key later. You provide the key value with the application ID to sign in as the application. Store the key value where your application can retrieve it.

### Required permissions
You must have sufficient permissions to register an application with your Azure AD tenant and assign the application to a role in your Azure subscription.
To do so, first check your Azure Active Directory permissions
Select the `Azure Active Directory` option.
Then select `User Settings` in the `Default Directory – Overview` section :

![@label](images/defaultdirectoryusersetting.png)

Next, check the App registrations setting. 
Note: This value can only be set by an administrator. If set to `Yes`, any user in the Azure AD tenant can register an app.

![@label](images/appregistrationyes.png)

If the app registrations setting is set to `No`, only users with an administrator role may register these types of applications. 

See available roles <https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-assign-admin-roles#available-roles> and role permissions <https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-assign-admin-roles#role-permissions> to learn about available administrator roles and the specific permissions in Azure AD that are given to each role. 

If your account is assigned to the `User` role, but the app registration setting is limited to admin users, you will need an administrator to either assign you to one of the `administrator` roles that can create and manage all aspects of app registrations, or to enable users to register apps.

### Check Azure subscription permissions
In your Azure subscription, your account must have `Microsoft.Authorization/*/Write` access to assign an AD app to a role. This action is granted through the Owner role or User Access Administrator role. 

If your account is assigned to the `Contributor` role, you do not have adequate permission. You will receive an error when attempting to assign the service principal to a role.

To check your subscription permissions:
Select the `All services` option under the `General` section, Then choose `Subscriptions`:

![@label](images/subscriptionsthree.png)

Choose the Subscription to assign the Application ID to. For example, my Subscription is `Azure for Students`:

![@label](images/azureforstudents.png)

Under the `Azure for Students` section, locate the `My Permissions` option:

![@label](images/mypermissions.png)

This will show your account permission. For Example:
![@label](images/resourceproviderstatus.png)

View your assigned roles and determine if you have adequate permissions to assign an AD app to a role. 
If not, an administrator will need to add you to the `User Access Administrator` role. In the following image, the user is assigned to the `User Access Administrator` role, which means that user has adequate permissions.

### Instantiating an Libcloud Azure ARM Compute Driver 
Use `<Application_Id>` for “key” and the `<Your_Password>` for “secret”.


Once you have the `tenant id`, `subscription id`, `application id (“key”)`, and `password (“secret”)`, you can create an AzureNodeDriver:

![@label](images/instantiatearmdriverdetail.png)

To interact with my Azure VMs, I have chosen the method to start a VM that is currently in a stopped state. 
This would be accomplished with the `ex_start_node` method listed in the ARM driver documentation :

![@label](images/exstartnode.png)

This is the Python code that I generated in order to test this method (Note: at the time of this writing this command did not return successfully)

```
from libcloud.compute.types import Provider
from libcloud.compute.providers import get_driver

cls = get_driver(Provider.AZURE_ARM)
driver = cls(tenant_id='<YourTenantIDHere>',
             subscription_id='<YourSubscriptionIDHere>',
             key='<YourKeyHere>',
             secret='<YourKeyHere>',
             region='centralus',
             )

#driver.ex_start_node()
driver.ex_start_node('TheNameOfMyLinuxVM')
```

## Microsoft Azure CLI

The Azure CLI is a command-line tool allows for interaction and management of Azure resources. The current version of the CLI is 2.0.63. For more information about releases please visit the following link <https://docs.microsoft.com/en-us/cli/azure/release-notes-azure-cli?view=azure-cli-latest>.
To find your installed version and see if you need to update, run `az --version`.

I will go through the installation and interaction on a Windows set up <https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest>, however you can find details about the Azure CLI setup on other machines here:<https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest>. 

### Install or update
The MSI distributable is used for installing, updating, and uninstalling the az command on Windows.

To download the installed click on the following:

![@label](images/downloadthemsaiinstaller.png)

When the installer asks if it can make changes to your computer, click the `Yes` box.

You can now run the Azure CLI with the az command from either Windows Command Prompt or PowerShell. PowerShell offers some tab completion features not available from Windows Command Prompt. To sign in, run the az login command.

Once you have installed the CLI and have been able to successfully login, you can reference the following steps to interact with an Azure environment with the CLI tool:
<https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli?view=azure-cli-latest>.

### Sign in
Before using any CLI commands with a local install, you need to sign in with az login.

![@label](images/azlogin.png)

You may be directed and prompted to provide your credentials through a web browser if you are not currently actively logged in already. Once your credentials have been provided successfully, you will receive a message as follows:

![@label](images/youhaveloggedin.png)

After logging in, you see a list of subscriptions associated with your Azure account. 
The subscription information with `isDefault: true` is the currently activated subscription after logging in. 

To select another subscription, use the `az account set` command with the subscription ID to switch to. 
For more information about subscription selection, see <https://docs.microsoft.com/en-us/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest>


#### Common commands
This following are some common commands used in the CLI:.

#### Resource group
`az group`

#### Virtual machines
`az vm`

#### Storage accounts
`az storage account`

#### Key Vault
`az keyvault`

#### Web applications
`az webapp`

#### SQL databases
`az sql server`

#### CosmosDB
`az cosmosdb`


### Finding commands
Commands in the CLI are organized as commands of groups. Each group represents an Azure service, and commands operate on that service.
To search for commands, use az find. For example, to search for command names containing secret, use the following command: `az find secret`:

![@label](images/azfindsecret.png)

Use the `—help` argument to get a complete list of commands and subgroups of a group. For example, to find the CLI commands for working with Network Security Groups (NSGs):

![@label](images/aznetworknsg.png)

The CLI has full tab completion for commands under the bash shell.

#### Globally available arguments
There are some arguments that are available for every command.
* `--help` prints CLI reference information about commands and their arguments and lists available subgroups and commands.
* `--output` changes the output format. The available output formats are json, jsonc (colorized JSON), tsv (Tab-Separated Values), table (human-readable ASCII tables), and yaml. By default the CLI outputs json. To learn more about the available output formats, see: <https://docs.microsoft.com/en-us/cli/azure/format-output-azure-cli?view=azure-cli-latest>
* `--query` uses the JMESPath query language to filter the output returned from Azure services. To learn more about queries, see <https://docs.microsoft.com/en-us/cli/azure/query-azure-cli?view=azure-cli-latest> and the `JMESPath tutorial` :<http://jmespath.org/tutorial.html>.
* `--verbose` prints information about resources created in Azure during an operation, and other useful information.
* `--debug` prints even more information about CLI operations, used for debugging purposes. If you find a bug, provide output generated with the --debug flag on when submitting a bug report.

#### Interactive mode
The CLI offers an interactive mode that automatically displays help information and makes it easier to select subcommands. You enter interactive mode with the az interactive command `az interactive`:

![@label](images/azinteractive.png)

For more information on interactive mode, see Azure CLI Interactive Mode: <https://docs.microsoft.com/en-us/cli/azure/interactive-azure-cli?view=azure-cli-latest?>

#### Learn CLI basics with quickstarts and tutorials
To get you started with the Azure CLI, try an in-depth tutorial for setting up virtual machines and using the power of the CLI to query Azure resources.

You can access the tutorial material at the following link:
<https://docs.microsoft.com/en-us/cli/azure/azure-cli-vm-tutorial?view=azure-cli-latest>

There are also quickstarts for other popular services.
* Create a storage account using the Azure CLI
* Transfer objects to/from Azure Blob storage using the CLI
* Create a single Azure SQL database using the Azure CLI
* Create an Azure Database for MySQL server using the Azure CLI
* Create an Azure Database for PostgreSQL using the Azure CLI
* Create a Python web app in Azure
* Run a custom Docker Hub image in Azure Web Apps for Containers


## Conclusion

TBD

## Acknowledgement

TBD
