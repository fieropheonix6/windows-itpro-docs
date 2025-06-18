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

# HTTPS Support for Linux

This article outlines how to configure HTTPS support your Microsoft Connected Cache for Enterprise and Education cache nodes.
placeholder text here.

## Overview

HTTPS support for Microsoft Connected Cache (MCC) enhances security and enables local delivery of Teams, Intune, and other content that require HTTPS downloads.

## Provision your MCC node with the new Installer

If you are using an existing cache node, you will need to re-run the installation. Please follow the steps below.

1. Ensure your Linux host machine is compliant with MCC prerequisites
2. Download and unzip the given file, WSLInstaller.zip, to your host machine
o Take note of this file path, you will run all new bash scripts from it
3. If you do not have an active MCC node, create one by following our public docs
o Make sure to indicate “Linux” OS on the cache node creation page
4. Complete the steps to deploy Connected Cache node to Linux, but do not download the provisioning package given in the Portal.
o You will be using the new Installer package that you unzipped earlier
5. On your host machine, run the provisioning command from the Installer directory location
o Please run the provisioning command as provided in your Azure Portal, this includes a list of credentials (customerid, cachenodeid, etc.)
6. Follow these steps to verify cache node functionality

## Update MCC container

1. Send your cache node ID to <adityamiddha@microsoft.com>
a. The engineering team will update the MCC container version to support port forwarding, TLS certificates, and other configurations for HTTPS
b. You will be emailed once your  cache node ID is unblocked
2. Once your ID is unblocked, check Azure Portal and verify that your MCC version is 2090-TLS before proceeding

## Generate a CSR

 1. On your Linux host, open a terminal and navigate to the lnstaller directory
 2. Add the correct permissions to the given bash script, ./generateCsr.sh
    a. Run chmod +x  generateCsr.sh
 3. Input required parameters for ./generateCsr.sh and then run the script. Parameter usage and examples in the table on the next page.
    o If you miss a required parameter, the script should alert you which parameters you missed
    o To test optional parameters not included in the given script, run the script with “-h” appended. Try various inputs for these optional parameters
    o If you encounter errors, locate the GenerateCSR.log file with the folder specified in the script output
        - Line starts with “You can find logs here: …”, should list the folder “…/var/mcc/certs/logs” in your MCC Installation folder
 4. Once the CSR Generation Process is completed, find the CSR in your Certificates folder (this is specified at the end of the script output)
    a. Output line starts with “CSR file created at: …”
    b. This folder should be in your MCC Install directory (parameter you specified in the provisioning script) under “…\Certificates\certs”
 5. Copy the CSR to the machine that you will be using to sign it

Required parameters:
  -algo, --algorithm    Certificate algorithm (RSA, EC, ED25519, ED448) [REQUIRED]
  -keySizeOrCurve, --keySizeOrCurve   Key size for RSA or curve for EC (e.g., 2048, prime256v1) [REQUIRED for RSA/EC]
  -csrName, --csrName   Name of the CSR [REQUIRED]

Subject options:
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

  -h, --help            Show this help message and exit

Examples:

    Full subject with multiple components

  ./generateCsr.sh  -algo RSA  -keySizeOrCurve 2048 -csrName myservercsr -subjectCountry "US" -subjectState "WA" -subjectOrg "MyOrg" -subjectCN "localhost" -sanDns "localhost,example.com" -sanIp "127.0.0.1,192.168.1.100"

    Minimal subject with CN only

  ./generateCsr.sh -algo EC -keySizeOrCurve prime256v1 -csrName webapp -subjectCommonName  "webapp.company.com" -sanDns "webapp.company.com, api.company.com"

## Sign the CSR

1. Select a public or enterprise Certificate Authority (CA) to use for signing the CSR. Please note, the CA signature must match a root certificate in the client’s trusted root store.
a. Common Public CAs to use (with links)
i. DigiCert
ii. Let’s Encrypt
2. Submit your CSR to the CA of your choice and save the resultant signed certificate
a. Signing requirements: .crt file type and X509 format
3. Move your signed certificate to the Certificates folder
a. In your MCC Install directory under “…\Certificates\certs”

## Import signed certificate

   1. On your Linux host, open a terminal and navigate to the location of the WSL Installer
   2. Add the correct permissions to the given bash script, ./importCert.sh
        a. Run chmod+x  importCert.sh
    3. Input the parameters and then run the script

    Required parameters:
      -certName, --certName   the entire file name of your signed TLS certificate, with or without the .crt at the end
    Example: 
    ./importCert.sh  -certName  myTlsCert.crt
    

## Validation

    Once the import process has completed, test HTTP and HTTPS content download using the following commands:
        a. curl.exe -v -o "nul" "<https://127.0.0.1/ee344de8-d177-4720-86c1-a076581766f9/070a8fd4-79a7-42c8-b7c8-9883253bb01a/c7b1b825-88b2-4e66-9b15-ff5fe0374bc6.appxbundle.bin>" --include -H "host:swda01-mscdn.manage.microsoft.com"
        b. curl.exe -v -o "nul" "<https://localhost/ee344de8-d177-4720-86c1-a076581766f9/070a8fd4-79a7-42c8-b7c8-9883253bb01a/c7b1b825-88b2-4e66-9b15-ff5fe0374bc6.appxbundle.bin>" --include -H "host:swda01-mscdn.manage.microsoft.com"
---

## Monitor TLS Certificate

placeholder text

## Disable TLS

    1. On your Linux host, open a command line window and navigate to the location of the WSL Installer 
    2. Add the correct permissions to the given bash script, ./disableTLS.sh, then run the script 
        a. Run chmod+x  disableTLS.sh
    3. Test HTTPS content download using the following commands:
        a. curl.exe -v -o "null" "https://localhost/ee344de8-d177-4720-86c1-a076581766f9/070a8fd4-79a7-42c8-b7c8-9883253bb01a/c7b1b825-88b2-4e66-9b15-ff5fe0374bc6.appxbundle.bin" --include -H "host:swda01-mscdn.manage.microsoft.com"
        b.  curl.exe -v -o "null" "http://localhost/ee344de8-d177-4720-86c1-a076581766f9/070a8fd4-79a7-42c8-b7c8-9883253bb01a/c7b1b825-88b2-4e66-9b15-ff5fe0374bc6.appxbundle.bin" --include -H "host:swda01-mscdn.manage.microsoft.com"

---
