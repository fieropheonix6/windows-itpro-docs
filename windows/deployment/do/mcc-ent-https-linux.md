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

placeholder text

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

## Sign the CSR

placeholder text

## Import signed certificate

   1. On your Linux host, open a terminal and navigate to the location of the WSL Installer
   2. Add the correct permissions to the given bash script, ./importCert.sh
        a. Run chmod+x  importCert.sh

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
