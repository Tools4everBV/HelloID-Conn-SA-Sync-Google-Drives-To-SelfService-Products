# HelloID-Conn-SA-Sync-Google-Drives-To-SelfService-Products
> [!IMPORTANT]
> **Best Practice - Maximum Synchronization Frequency: Once per day**
>
> **Why this maximum?**
> New resources are typically created daily or weekly, not hourly. More frequent synchronization causes unnecessary API calls and processing load on both source systems and HelloID without business value.
>
> If a higher frequency is required for your organization, please contact **Tools4ever Support**. This helps us understand your use case and provide proper guidance.

> [!IMPORTANT]
> This repository contains the script and configuration code only. The implementer is responsible to acquire the connection details such as username, password, certificate, etc. You might even need to sign a contract or agreement with the supplier before implementing this script. Please contact the client's application manager to coordinate the script requirements.

<p align="center">
  <img src="https://github.com/Tools4everBV/HelloID-Conn-SA-Sync-Google-Drives-To-SelfService-Products/blob/main/Logo.png?raw=true">
</p>

## Table of contents

- [HelloID-Conn-SA-Sync-Google-Drives-To-SelfService-Products](#helloid-conn-sa-sync-google-drives-to-selfservice-products)
  - [Table of contents](#table-of-contents)
  - [Requirements](#requirements)
  - [Remarks](#remarks)
  - [Introduction](#introduction)
      - [Description](#description)
      - [Endpoints](#endpoints)
  - [Script Setup](#script-setup)
    - [Create a Google Cloud project](#create-a-google-cloud-project)
    - [Enable Google Workspace APIs](#enable-google-workspace-apis)
    - [Create access credentials](#create-access-credentials)
      - [Create a service account](#create-a-service-account)
    - [Create credentials for a service account](#create-credentials-for-a-service-account)
    - [Set up domain-wide delegation for a service account](#set-up-domain-wide-delegation-for-a-service-account)
    - [User Defined Variables](#user-defined-variables)
  - [HelloID docs](#helloid-docs)

## Requirements
1. **HelloID Environment**:
   - Set up your _HelloID_ environment.
   - Install the _HelloID_ Service Automation agent (cloud or on-prem).
2. **Google API Credentials**:
   - Create a project in the Google Cloud Console.
   - Enable the Admin SDK API for your project.
   - Enable the Gmail API for your project.
   - Create access credentials for your project:
     - Create a service account for your project.
     - Generate credentials (P12 key) for your service account.
     - Set up domain-wide delegation for your service account with the following scopes:
       - `https://www.googleapis.com/auth/drive`

## Remarks
- None at this time.

## Introduction

#### Description
_HelloID-Conn-SA-Sync-Google-Drives-To-SelfService-Products_ is a template designed for use with HelloID Service Automation (SA) Tasks. It can be used to schedule a synchronization of Google drives to products.

#### Endpoints
Google Workspace provides a set of REST APIs that allow you to programmatically interact with its data.. The API endpoints listed in the table below are used.

| Endpoint                                                                                                                                                 | Description                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| [https://www.googleapis.com/auth/admin.directory.group]
| [https://www.googleapis.com/auth/admin.directory.user]
| [https://www.googleapis.com/auth/drive]
| [https://www.googleapis.com/drive/v3/drives]


## Script Setup
### Create a Google Cloud project
The first step to connect to the Google API and make requests is to register a new **Google Cloud project**. This project is required to use Google Workspace APIs.

Follow these steps:
1. Go to the Google Cloud Console.
2. Create a new project.

For more information, please see: [Create a Google Cloud project](https://developers.google.com/workspace/guides/create-project).

### Enable Google Workspace APIs
Next, enable the necessary Google Workspace APIs in your Google Cloud project. For this connector, we use the **Admin SDK API** and the **Drive API**.

Follow these steps:

1. In your Google Cloud project, navigate to the “APIs & Services” > “Library” section.
2. Search for “Admin SDK” and enable it.
3. Search for “Drive API” and enable it.

For more information, please see: [Enable Google Workspace APIs](https://developers.google.com/workspace/guides/enable-apis).

### Create access credentials
There are multiple ways to authenticate to the Google API, but we use (and recommend) the service account credentials.

#### Create a service account
A service account is a special kind of account used by an application (rather than a person). You can use a service account to access data or perform actions on behalf of Google Workspace or Cloud Identity users.

Follow these steps:

1. In your Google Cloud project, navigate to the “APIs & Services” > “Credentials” section.
2. Create a new service account.
3. Note down the service account email address.

For more information, please see: [Create a service account](https://developers.google.com/workspace/guides/create-credentials#create_a_service_account).

### Create credentials for a service account
To authorize service account actions within your app, you need to obtain credentials in the form of a public/private key pair. We recommend using the P12 key type.

Follow these steps:

1. In the “Credentials” section, create a new key for your service account.
2. Choose the P12 key type.
3. Download the P12 key file and securely store it.

For more information, please see: [Create credentials for a service account](https://developers.google.com/workspace/guides/create-credentials#create_credentials_for_a_service_account).

### Set up domain-wide delegation for a service account
To call APIs on behalf of users in a Google Workspace organization, your service account needs to be granted domain-wide delegation of authority in the Google Workspace Admin console by a super administrator account.

The following scopes are required:

- `https://www.googleapis.com/auth/admin.directory.group`
- `https://www.googleapis.com/auth/admin.directory.user`
- `https://www.googleapis.com/drive/v3/drives`
- `https://www.googleapis.com/auth/drive`

For more information, please see: [Set up domain-wide delegation for a service account](https://developers.google.com/workspace/guides/create-credentials#optional_set_up_domain-wide_delegation_for_a_service_account).

### User Defined Variables
The following user-defined variables are used by the script. Ensure that you check and set the correct values required to connect to the API.

| Setting                            | Description                                                                                                                                                               |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$serviceAccountEmail`        | The email address of your service account                                                                                                                                 |
| `$p12CertificateBase64`       | The Base64 string of the P12 certificate of your service account                                                                                                          |
| `$p12CertificatePassword`     | The password to the P12 certificate of your service account                                                                                                               |
| `$userId`                 | The email address of an admin account with permissions to manage users                                                                                                    |

**Note:** The Base64 string of the P12 certificate file can be obtained by running the following code:
```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("C:\path\to\your\certificate.p12"))
```

## HelloID docs
The official HelloID documentation can be found at: https://docs.helloid.com/