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

## Setup - Student Azure Portal
To start working in the Azure environment, you will to first need to set up an Azure Student Account. 
You can set up and activate an Azure student account at <https://azure.microsoft.com/en-us/free/students/>. 
The Azure student account FAQ will likely answer questions you might have pertaining to an Azure Student Account, what you will have access to, how long you will enjoy access, and additional general overview information including terms of the account. <https://azure.microsoft.com/en-us/free/free-account-students-faq/>

Once you have set up the Azure Student account, you will gain access the Azure environment through the Azure Portal <https://portal.azure.com>. To log in, please use the credentials you determined during the set up.

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
