---
title: Configure HTTPS Support for Windows
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

## Install latest deployment package

If you don't have an active Connected Cache node, create one by following these instructions [Link text](http://ask.fm). When you install Connected Cache, your deployment package will have the new Installer.

If you are using an existing cache node, **you will need to reinstall the deployment package on your cache node**. Skip the create and configure step, complete deployment instructions.

## Generate a Certificate Signing Request (CSR)

 1. On your Windows host, open a PowerShell terminal and navigate to the Installer directory
 2. Input parameters for the given PowerShell script, .\generateCsr.ps1, and then run the script.

    - If you miss a required parameter, the script should alert you which parameters you missed
    - If you encounter errors, locate the GenerateCSR.log file with the folder specified in the script output. The output line starts with "You can find logs here: …"

    ### Generate CSR Script Parameters

    #### Required Parameters

    **`-algo` / `--algorithm`** *(Required)*  
    Certificate algorithm options: `RSA`, `EC`, `ED25519`, `ED448`

    **`-keySizeOrCurve` / `--keySize`** *(Required for RSA/EC)*  
    - For RSA: Key size like `2048`, `3072`, `4096`
    - For EC: Curve name like `prime256v1`, `secp384r1`

    **`-csrName` / `--csrName`** *(Required)*  
    Name for the generated CSR file

    **`-RunTimeAccountName` / `--RunTimeAccountName`** *(Required)*  
    Username from your PowerShell credential object

    **`-LocalAccountCredential` / `--LocalAccountCredential`** *(Required)*  
    Complete PowerShell credential object

    #### Subject Parameters

    **`-subjectCommonName` / `--subjectCommonName`** *(Required)*  
    Common name for the certificate  
    Examples: `"localhost"`, `"example.com"`

    **`-subjectCountry` / `--subjectCountry`** *(Optional)*  
    Two-letter country code  
    Examples: `"US"`, `"CA"`, `"GB"`

    **`-subjectState` / `--subjectState`** *(Optional)*  
    State or province  
    Examples: `"WA"`, `"TX"`, `"Ontario"`

    **`-subjectOrg` / `--subjectOrg`** *(Optional)*  
    Organization name  
    Examples: `"MyCompany"`, `"ACME Corp"`

    #### Subject Alternative Names (At least one required)

    **`-sanDns` / `--sanDns`**  
    DNS names (comma-separated)  
    Example: `"localhost,example.com,api.example.com"`

    **`-sanIp` / `--sanIp`**  
    IP addresses (comma-separated)  
    Example: `"127.0.0.1,192.168.1.100"`

    **`-sanUri` / `--sanUri`**  
    URIs (comma-separated)  
    Example: `"https://example.com, http://localhost"`

    **`-sanEmail` / `--sanEmail`**  
    Email addresses (comma-separated)  
    Example: `"admin@example.com,user@domain.com"`

    #### Examples

    **Full Certificate with Multiple Components**

    ```powershell
    .\generateCsr.ps1 `
      -RunTimeAccountName $myLocalAccountCredential.Username `
      -LocalAccountCredential $myLocalAccountCredential `
      -algo RSA `
      -keySize 2048 `
      -csrName "myservercsr" `
      -subjectCountry "US" `
      -subjectState "WA" `
      -subjectOrg "MyOrg" `
      -subjectCommonName "localhost" `
      -sanDns "localhost,example.com" `
      -sanIp "127.0.0.1,192.168.1.100"
    ```

    **Minimal Certificate with Common Name Only**

    ```powershell
    .\generateCsr.ps1 `
      -RunTimeAccountName $myLocalAccountCredential.Username `
      -LocalAccountCredential $myLocalAccountCredential `
      -algo EC `
      -keySize prime256v1 `
      -csrName "webapp" `
      -subjectCommonName "webapp.company.com" `
      -sanDns "webapp.company.com,api.company.com"
    ```

    **RSA Certificate with Email SAN**

    ```powershell
    .\generateCsr.ps1 `
      -RunTimeAccountName $myLocalAccountCredential.Username `
      -LocalAccountCredential $myLocalAccountCredential `
      -algo RSA `
      -keySize 3072 `
      -csrName "emailcert" `
      -subjectCommonName "John Doe" `
      -subjectOrg "ACME Corporation" `
      -sanEmail "john.doe@acme.com,admin@acme.com"
    ```

 3. Once the CSR Generation Process is completed, find the CSR in your Certificates folder (location is specified at the end of the script output)

    - Output line starts with "CSR file created at: …"
    - This folder should be in your install scripts folder under "…\Certificates\certs\"

 4. Copy the CSR to the machine that you will be using to sign it

## Sign the CSR

1. Select a public or enterprise Certificate Authority (CA) to use for signing the CSR. The CA signature must match a root certificate in the client’s trusted root store.
    - Common Public CAs to use: DigiCert, Let's Encrypt
2. Submit your CSR to the CA of your choice and save the resultant signed certificate
    - Signing requirements: .crt file type and X509 format
3. Move your signed certificate to the Certificates folder
    - In your Install directory, place under "…\Certificates\certs\"

## Import signed TLS certificate

1. Open a PowerShell terminal and navigate to the location of the WSL Installer
2. Input the parameters of the given PowerShell script, importCert.ps1, and then run the script.

   ### Parameters

    **`-certName` / `--certName`** *(Required)*  
    The complete filename of your signed TLS certificate  
    Examples: `"myTlsCert.crt"`, `"server.crt"`, `"webapp-cert"`  
    *Note: Include or omit the .crt extension - both work*

    **`-RunTimeAccountName` / `--RunTimeAccountName`** *(Required)*  
    Username from your PowerShell credential object  
    Example: `$myLocalAccountCredential.Username`

    **`-LocalAccountCredential` / `--LocalAccountCredential`** *(Required)*  
    Complete PowerShell credential object  
    Example: `$myLocalAccountCredential`

   ### Example

    **Import a TLS certificate:**

    ```powershell
    .\importCert.ps1 `
      -RunTimeAccountName $myLocalAccountCredential.Username `
      -LocalAccountCredential $myLocalAccountCredential `
      -certName "myTlsCert.crt"

## Validation

Once the import process completes, test HTTP and HTTPS content downloads using the following commands:

```powershell
# Test HTTPS
curl -v -o $null "https://localhost/ee344de8-d177-4720-86c1-a076581766f9/070a8fd4-79a7-42c8-b7c8-9883253bb01a/c7b1b825-88b2-4e66-9b15-ff5fe0374bc6.appxbundle.bin" --include -H "host:swda01-mscdn.manage.microsoft.com"

# Test HTTP
curl -v -o $null "http://localhost/ee344de8-d177-4720-86c1-a076581766f9/070a8fd4-79a7-42c8-b7c8-9883253bb01a/c7b1b825-88b2-4e66-9b15-ff5fe0374bc6.appxbundle.bin" --include -H "host:swda01-mscdn.manage.microsoft.com"
```

## Monitor TLS certificate

Ability to monitor the  status (active/inactive, expiry date) of your TLS Certificate will soon be available in the Azure portal.

## Remove TLS certificate

1. Open a PowerShell terminal and navigate to the location of the WSL Installer
2. From this folder, run .\disableTLS.ps1

   ### .\disableTLS.ps1 parameters

     **`-RunTimeAccountName` / `--RunTimeAccountName`** *(Required)*  
    Username from your PowerShell credential object  
    Example: `$myLocalAccountCredential.Username`

    **`-LocalAccountCredential` / `--LocalAccountCredential`** *(Required)*  
    Complete PowerShell credential object  
    Example: `$myLocalAccountCredential`

   ### .\disableTLS.ps1 example

    ```powershell
      .\disableTLS.ps1 `
      -RunTimeAccountName $myLocalAccountCredential.Username `
      -LocalAccountCredential $myLocalAccountCredential `
    ```

3. Once the disable process completes, test HTTP and HTTPS (should no longer work) content downloads using the following commands:

    ```powershell
    # Test HTTPS
    curl -v -o $null "https://localhost/ee344de8-d177-4720-86c1-a076581766f9/070a8fd4-79a7-42c8-b7c8-9883253bb01a/c7b1b825-88b2-4e66-9b15-ff5fe0374bc6.appxbundle.bin" --include -H "host:swda01-mscdn.manage.microsoft.com"
    
    # Test HTTP
    curl -v -o $null "http://localhost/ee344de8-d177-4720-86c1-a076581766f9/070a8fd4-79a7-42c8-b7c8-9883253bb01a/c7b1b825-88b2-4e66-9b15-ff5fe0374bc6.appxbundle.bin" --include -H "host:swda01-mscdn.manage.microsoft.com"
    ```

---
