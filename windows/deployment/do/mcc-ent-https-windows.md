---
title: Configure HTTPS Support for Cache nodes
description: Details on how to configure HTTPS support Microsoft Connected Cache for Enterprise and Education cache nodes.
ms.service: windows-client
ms.subservice: itpro-updates
ms.topic: how-to
manager: naengler
ms.author: adityamiddha
author: adityamiddha
appliesto: 
- ✅ <a href=https://learn.microsoft.com/windows/release-health/supported-versions-windows-client target=_blank>Windows 11</a>
- ✅ Supported Linux distributions
- ✅ <a href=https://learn.microsoft.com/windows/deployment/do/waas-microsoft-connected-cache target=_blank>Microsoft Connected Cache for Enterprise</a> 
ms.date: 06/13/2025
---

# HTTPS Support for Windows

This article outlines how to configure HTTPS support your Microsoft Connected Cache for Enterprise and Education cache nodes.
placeholder text here.

## Overview

HTTPS support for Microsoft Connected Cache (MCC) enhances security and enables local delivery of Teams, Intune, and other content that require HTTPS downloads.

## Provision your MCC with the new Installer

1. Ensure your Windows host machine is compliant with MCC prerequisites
2. Download and unzip the given file, WSLInstaller.zip, to your host machine
    - The installer should be in a folder not synced to OneDrive; we recommend using your C drive
    - Take note of this file path, you will run all new PowerShell scripts from it
3. If you do not have an active MCC node, create one by following our public docs
    - Make sure to indicate “Windows” OS on the cache node creation page
4. Complete the steps to deploy Connected Cache cache node to Windows, but do not download the provisioning package given in the Portal.
    - You will be using the new Installer package that you unzipped earlier
5. On your host machine, run the provisioning command from the Installer directory location
    - Please run the provisioning command as provided in your Azure Portal, this includes a list of credentials (customerid, cachenodeid, etc.)
6. Follow these steps to verify cache node functionality

## Generate a CSR

1. On your Windows host, open a PowerShell terminal and navigate to the Installer directory
2. Input required parameters for the given PowerShell script, .\generateCsr.ps1, and then run the script. Parameter usage and examples in the table below.

    - If you miss a required parameter, the script should alert you which parameters you missed
    - To test optional parameters not included in the given script, run the script with “-h” appended. Try various inputs for these optional parameters
    - If you encounter errors, locate the GenerateCSR.log file with the folder specified in the script output. Email this to the Adi and Bhavya, the contacts listed in step 2.
    - Line starts with “You can find logs here: …”, should list the folder “…\Certificates\logs” in your MCC Installation folder

3. Once the CSR Generation Process is completed, find the CSR in your Certificates folder (this is specified at the end of the script output)

- Output line starts with “CSR file created at: …”
- This folder should be in your install scripts folder under “…\Certificates\certs\”

Required parameters:

| Parameter Name | Description | Required? |
|---|---|---|
|-algo </br></br> --algorithm | Certificate algorithm (RSA, EC, ED25519, ED448) | [REQUIRED] |
|-keySizeOrCurve </br></br> --keySize | Key size for RSA (e.g. 2048, 3072) or curve for EC (e.g. prime256v1) | [REQUIRED if using RSA or EC] |
|-csrName </br></br> --csrName | Name of the CSR | [REQUIRED] |
|-RunTimeAccountName </br></br> --RunTimeAccountName | the username of your PS credential object | [REQUIRED] |
|-LocalAccountCredential </br></br> --LocalAccountCredential |  the complete PS credential object | [REQUIRED] |

Subject options [only CommonName is REQUIRED]:
  -subjectCountry, --subjectCountry Subject country code (optional, e.g., "US")
  -subjectState, --subjectState     Subject state/province (optional, e.g., "WA")
  -subjectOrg, --subjectOrg         Subject organization (optional, e.g., "MyOrg")
  -subjectCommonName, --subjectCommonName    Subject common name ([REQUIRED], e.g., "localhost")
SAN (Subject Alternative Name) options [At least one REQUIRED]:
  -sanDns, --sanDns     DNS names (comma-separated, e.g., "localhost,example.com")
  -sanIp, --sanIp       IP addresses (comma-separated, e.g., "127.0.0.1,192.168.1.100")
  -sanUri, --sanUri     URIs (comma-separated, e.g., "<https://example.com,http://localhost>")
  -sanEmail, --sanEmail Email addresses (comma-separated, e.g., "<admin@example.com>,<user@domain.com>")
  -sanRid, --sanRid     Registered IDs (comma-separated)
  -sanDirName, --sanDirName Directory names (comma-separated)
  -sanOtherName, --sanOtherName Other names (comma-separated)

Examples:

    Full subject with multiple components

  .\generateCsr.ps1 -RunTimeAccountName $myLocalAccountCredential.Username                                   -LocalAccountCredential $myLocalAccountCredential  -algo RSA  -keySize 2048 -csrName myservercsr -subjectCountry "US" -subjectState "WA" -subjectOrg "MyOrg" -subjectCN "localhost" -sanDns "localhost,example.com" -sanIp "127.0.0.1,192.168.1.100"

    Minimal subject with CN only

  .\generateCsr.ps1 -RunTimeAccountName $myLocalAccountCredential.Username -LocalAccountCredential $myLocalAccountCredential -algo EC -keySize prime256v1 -csrName webapp -subjectCN "webapp.company.com" -sanDns "webapp.company.com,api.company.com"

## Sign the CSR

1. Select a public or enterprise Certificate Authority (CA) to use for signing the CSR. Please note, the CA signature must match a root certificate in the client’s trusted root store.
    - Common Public CAs to use: DigiCert, Let's Encrypt
2. Submit your CSR to the CA of your choice and save the resultant signed certificate
    - Signing requirements: .crt file type and X509 format
3. Move your signed certificate to the Certificates folder
    - In your MCC Install directory, place under “…\Certificates\certs\”

## Import signed certificate

1. Open a PowerShell terminal and navigate to the location of the WSL Installer
2. Input the parameters of the given PowerShell script, importCert.ps1, and then run the script.

Required parameters:

| Parameter Name | Description | Required? |
|------|---|---|
|-certName </br></br> --certName| the entire file name of your signed TLS certificate, with or without the .crt at the end | Yes |
|-RunTimeAccountName </br></br> --RunTimeAccountName|  the username of your PS credential object | Yes |
|-LocalAccountCredential </br></br> --LocalAccountCredential|  the complete PS credential object | Yes |

Example:
.\importCert.ps1  -RunTimeAccountName $myLocalAccountCredential.Username   -LocalAccountCredential $myLocalAccountCredential  -certName  myTlsCert.crt

## Validation

Once the import process has completed, test HTTP and HTTPS content download using the following commands:
    • curl.exe -v -o "null" "<https://localhost/ee344de8-d177-4720-86c1-a076581766f9/070a8fd4-79a7-42c8-b7c8-9883253bb01a/c7b1b825-88b2-4e66-9b15-ff5fe0374bc6.appxbundle.bin>" --include -H "host:swda01-mscdn.manage.microsoft.com"
    •  curl.exe -v -o "null" "<http://localhost/ee344de8-d177-4720-86c1-a076581766f9/070a8fd4-79a7-42c8-b7c8-9883253bb01a/c7b1b825-88b2-4e66-9b15-ff5fe0374bc6.appxbundle.bin>" --include -H "host:swda01-mscdn.manage.microsoft.com"

---

## Monitor TLS Certificate

placeholder text

---

## Disable TLS

1. Open a PowerShell terminal and navigate to the location of the WSL Installer
2. From this folder, run .\disableTLS.ps1

Required parameters:
 -RunTimeAccountName, --RunTimeAccountName   the username of your PS credential object [REQUIRED]
  -LocalAccountCredential, --LocalAccountCredential   the complete PS credential object [REQUIRED]

Example:
.\disableTLS.ps1  -RunTimeAccountName $myLocalAccountCredential.Username   -LocalAccountCredential $myLocalAccountCredential

3. Once the disable process is completed, test HTTP and HTTPS (should no longer work) content downloads using the following commands:
    a. curl.exe -v -o "null" "<https://localhost/ee344de8-d177-4720-86c1-a076581766f9/070a8fd4-79a7-42c8-b7c8-9883253bb01a/c7b1b825-88b2-4e66-9b15-ff5fe0374bc6.appxbundle.bin>" --include -H "host:swda01-mscdn.manage.microsoft.com"
    b.  curl.exe -v -o "null" "<http://localhost/ee344de8-d177-4720-86c1-a076581766f9/070a8fd4-79a7-42c8-b7c8-9883253bb01a/c7b1b825-88b2-4e66-9b15-ff5fe0374bc6.appxbundle.bin>" --include -H "host:swda01-mscdn.manage.microsoft.com

---
