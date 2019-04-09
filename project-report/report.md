# Azure VM Management


| Andrew Garbe
| agarbe@iu.edu
| Indiana University
| hid: sp19-516-124
| github: [:cloud:](https://github.com/cloudmesh-community/sp19-516-124/edit/master/project-report/report.md)

## Abstract

TBD

## Introduction

TBD

## Requirements

TBD

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
ex_create_cloud_service(name, location, description=None, extended_properties=None)
driver.ex_create_cloud_service('e503CloudServicetest', 'Central US', description=None, extended_properties=None)
```

Once the `ex_create_cloud_service` Python code has been executed, I am now able to the `e503CloudServicetest` cloud service in the Azure Portal under `All resources`:   

![@label](images/e503cloudservicetest.png)



## Apache Libcloud Azure ARM Compute Driver

TBD

## Microsoft Azure CLI

TBD

## Azure SDK for Python

TBD

## Conclusion

TBD

## Acknowledgement

TBD
