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

# Configure HTTPS Support for Cache nodes

This article outlines how to configure HTTPS support your Microsoft Connected Cache for Enterprise and Education cache nodes.
placeholder text here.

## Overview

With the GA release version of Microsoft Connected Cache for Enterprise, your cache node can now deliver using HTTPS. This feature will allow your node to continue delivering Intune-managed win32 apps and newly deliver Microsoft Teams content. Along with these content types, we expect more publishers to have HTTPS requirements in the near future, so it is important to configure your node for HTTPS support as soon as possible.
To set up HTTPS delivery, your cache node will generate a Certificate Signing Request (CSR) for you to sign using a Certificate Authority (CA) and re-upload. We have outlined instructions and scripts for both Windows and Linux host machines to accomplish this.

## Change

Previously, MCC could content to clients requesting HTTP URLs only. If a client were to request an HTTPS URL, MCC would reject the request, and the client would immediately redirect to delivering via CDN. This is in large part because MCC did not store TLS certificate or port 443 configuration to initiate an HTTPS connection.

Without HTTPS, MCC could still deliver securely with methods such as hash validation and container hardening to protect against third party threats. But with some publishers making their content HTTPS-exclusive, MCC has now added HTTPS support to ensure customers continue to have access to new and existing content types.

## Benefits of HTTPS support on MCC

We recommend setting up HTTPS support on your cache node to receive the following benefits:

- Microsoft Teams content (not available via HTTP)
- Continue to receive managed win32 apps after Intune enforces HTTPS delivery
- Avoid failover to CDN for HTTPS URLs, which increases networking costs and limits bandwidth
- Improved security of MCC content delivery

> [!IMPORTANT]
> Intune will require all managed Win32 apps to be delivered via HTTPS starting November 6th, 2025. Thus, all Intune customers using MCC will have to complete the HTTPS setup process on their cache nodes to continue leveraging MCC to deliver Intune content.
>
> Hybrid / SCCM customers will have an alternate process; details are being discussed

## Impact of dual-delivery on MCC

With HTTPS support established, your MCC cache node will have the ability to use either HTTP or HTTPS for content delivery; the protocol will ultimately be determined by the URL that the DO client requests. For now, only Teams content will be requested using an HTTPS URL, but we expect Intune and other publishers to follow soon.

Importantly, this dual-delivery capability has no impact on the download experience for the client. As tested, performance is not affected, and if either method is not working correctly, MCC will still instantly fail over to CDN. Downloads directly from CDN will also not be impacted if HTTPS support is enabled; P2P with DO will continue to work.

## How it works

HTTPS uses cryptogrophy to ensure 3 universal traits; that data exchanged between two parties has not been tampered with (integrity), cannot be read by any external party (confidentiality), and is from the intended user (trust).
In the HTTPS communication between your client device and your MCC node, your cache node is acting as a server. Your client device will request a secure connection with your cache node, to which your cache node must provide a CA-signed certificate to validate its identity. Once your client device validates the certificate against its pre-existing certificate store, it will create a symmetric “session key” to more easily encrypt/decrypt further communication. A secure connection is established upon verifying the session key.

:::image type="content" source="./images/csr_workflow.png" alt-text="Diagram displaying how CSR generation works." lightbox="./images/csr_workflow.png":::

*paragraph about why we decided to do CSR process*
*why we can’t automate it*

1. Generate Certificate Signing Request (CSR)
    - Many encryption algorithms and key sizes – we have recommendations
    - Authentication Code – what does it make sure of
    - Subject / SAN – what’s their usage?
    - OpenSSL backend to generate key pair (stored in container) and CSR after
    - Exporting
2. Sign CSR
    - Requirements: .crt and X509 format
3. Import signed TLS Certificate
    - Authentication Code – what does it make sure

## Certificate maintenance on MCC

    TLS certificates require consistent maintenance, as they often expire or are revoked. To ease this process on MCC, we ha
    Available on portal soon
    - Watch for revocation and expiry – will be automated in future (add blue info bubble on this)
    - Certificate retention policy – active certs stored for duration, deactivated certs will be stored for 18 months after they have been deactivated or expired

## Next step

### [Azure portal](#tab/portal)

To set up HTTPS support on a **Linux** host machine, see
>[!div class="nextstepaction"]
>[HTTPS setup on Linux](mcc-ent-https-linux.md)

To set up HTTPS support on a **Windows** host machine, see
>[!div class="nextstepaction"]
>[HTTPS setup on Windows](mcc-ent-https-windows.md)

Watch out for additional guidance on setting up HTTPS support with CLI or in a proxy environment

---
