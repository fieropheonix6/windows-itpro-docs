---
title: HTTPS Support for MCC Overview
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

# HTTPS Support for Microsoft Connected Cache Overview

This article outlines how to configure HTTPS support your Connected Cache for Enterprise and Education cache nodes.

## Overview

With the GA release version of Connected Cache for Enterprise, your cache node can now deliver using HTTPS. This feature allows your node to continue delivering Intune-managed Win32 apps and newly deliver Microsoft Teams content. Along with these content types, we expect more publishers to have HTTPS requirements soon, so it's important to configure your node for HTTPS support as soon as possible.
To set up HTTPS delivery, your cache node generates a Certificate Signing Request (CSR) for you to sign using a Certificate Authority (CA) and reupload back to your Connected Cache. We have instructions and scripts for both Windows and Linux host machines to help guide this process.

*Must do for every cache node!!!*

## Change

Previously, Connected Cache could deliver content to clients requesting HTTP URLs only. If a client requested an HTTPS URL, Connected Cache would reject the request, and the client immediately redirected to CDN. With no TLS certificate or port 443 configuration, Connected Cache had no way to initiate an HTTPS connection.

Without HTTPS, Connected Cache could still deliver securely with methods such as hash validation and container hardening to protect against external threats. But with some publishers making their content HTTPS-exclusive, Connected Cache now supports HTTPS delivery to ensure customers continue to have access to new and existing content types.

## Benefits of HTTPS support on Connected Cache

We recommend setting up HTTPS support on your cache node to receive the following benefits:

- Microsoft Teams content (not available via HTTP)
- Continue to receive managed Win32 apps after Intune enforces HTTPS delivery
- Avoid failover to CDN for HTTPS URLs, which increases networking costs and limits bandwidth
- Improved security of content delivery

> [!IMPORTANT]
> Intune will require all managed Win32 apps to be delivered via HTTPS starting November 6, 2025. Thus, all Intune customers using Connected Cache must complete the HTTPS setup process on their cache nodes to continue using Connected Cache to deliver Intune content.
>
> Hybrid / SCCM customers will have an alternate process; details are being discussed

## Dual-delivery on Connected Cache

With HTTPS support established, your cache node will have the ability to use either HTTP or HTTPS for content delivery; the URL that the DO client requests determines the protocol. For now, only Teams content is requested using an HTTPS URL, but we expect Intune and other publishers to follow soon.

Importantly, this dual-delivery capability has no effect on the download experience for the client. As tested, performance is not affected, and if either method is not working correctly, Connected Cache fails over to CDN. Downloads directly from CDN won't be affected if HTTPS support is enabled; P2P with DO will continue to work.

## How it works

In the HTTPS communication between your client device and your Connected Cache, your cache node is acting as a server. Your client device requests a secure connection with your cache node, to which your cache node must provide a CA-signed certificate to validate its identity. Once your client device validates the certificate against its preexisting certificate store, it creates a symmetric "session key" to more easily encrypt/decrypt further communication. A secure connection is established upon verifying the session key.

:::image type="content" source="./images/ent-mcc-https-walkthrough.png" alt-text="Diagram displaying how CSR generation works." lightbox="./images/ent-mcc-https-walkthrough.png":::

*Paragraph about why we decided to do CSR process*
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

## Certificate maintenance on Connected Cache

TLS certificates require consistent maintenance, as they often expire or are revoked. To ease this process on Connected Cache, we have:

Available on portal soon

Watch for revocation and expiry – will be automated in future (add blue info bubble on this)

Certificate retention policy – active certs stored for duration, deactivated certs will be stored for 18 months after they have been deactivated or expired

## Next step

### [Azure portal](#tab/portal)

To set up HTTPS support on a **Linux** host machine, see
>[!div class="nextstepaction"]
>[HTTPS setup on Linux](mcc-ent-https-linux.md)

To set up HTTPS support on a **Windows** host machine, see
>[!div class="nextstepaction"]
>[HTTPS setup on Windows](mcc-ent-https-windows.md)

Watch out for more guidance on setting up HTTPS support with CLI or in a proxy environment

---
