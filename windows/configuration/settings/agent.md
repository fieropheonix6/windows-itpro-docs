---
title: Configure the Settings agent in Windows
description: Learn how to configure the Settings agent in Windows.
ms.topic: how-to
ms.date: 05/05/2025
author: paolomatarazzo
ms.author: paoloma
appliesto:
  - "✅ <a href=\"https://learn.microsoft.com/windows-insider/flight-hub\" target=\"_blank\">Windows Insider (Beta Channel)</a>"
---

# Configure the Settings agent in Windows

[!INCLUDE [insider-feature](../includes/insider-feature.md)]

Starting with [Windows Insider 22635.xxxx (Beta Channel)][KB-1], the Settings agent is a new feature in Windows that uses on-device AI to help you find and change settings on your PC. It can also help you troubleshoot issues by providing recommendations and automating tasks based on your input.

The Settings agent is designed to be a helpful assistant that can guide users through the process of finding and changing settings on thier PCs. Users can ask the agent questions or describe what they need help with, and it will provide relevant information and recommendations. Once the user has provided their permission, the agent can even automate the process of changing settings on their behalf.

## System requirements

Here's a list of requirements to use the Settings agent:

> [!div class="checklist"]
> - Windows Insider 22635.xxxx (Beta Channel) and later
> - A [Copilot+ PC](https://aka.ms/copilotpluspcs)

## Settings agent policy setting

As an administrator, you can control the Settings agent's visibility and functionality.

## Configuration

[!INCLUDE [tab-intro](../../../includes/configure/tab-intro.md)]

#### [:::image type="icon" source="../images/icons/intune.svg" border="false"::: **Intune**](#tab/intune)

[!INCLUDE [intune-settings-catalog-1](../../../includes/configure/intune-settings-catalog-1.md)]

| Category | Setting name | Value |
|--|--|--|
| **Settings** | - Page Visibility List<br>- Page Visibility List (User)| List of URIs to show or hide, separated by semicolons.|

[!INCLUDE [intune-settings-catalog-2](../../../includes/configure/intune-settings-catalog-2.md)]

#### [:::image type="icon" source="../images/icons/csp.svg"::: **CSP**](#tab/csp)

You can configure devices using the [Policy CSP][CSP-1].

| Setting |
|--|
|- **OMA-URI:** `./Device/Vendor/MSFT/Policy/Config/Settings/PageVisibilityList`<br>- **Data type:** string<br>- **Value:** List of URIs to show or hide, separated by semicolons.<br><br>Or<br><br>- **OMA-URI:** `./User/Vendor/MSFT/Policy/Config/Settings/PageVisibilityList`<br>- **Data type:** string<br>- **Value:** List of URIs to show or hide, separated by semicolons.|

#### [:::image type="icon" source="../images/icons/group-policy.svg" border="false"::: **GPO**](#tab/gpo)

[!INCLUDE [gpo-settings-1](../../../includes/configure/gpo-settings-1.md)]

| Group policy path | Group policy setting | Value |
| - | - | - |
| **Computer Configuration\Administrative Templates\Control Panel**<br><br>Or<br><br>**User Configuration\Administrative Templates\Control Panel** | Settings Page Visibility | List of URIs to show or hide, separated by semicolons.|

[!INCLUDE [gpo-settings-2](../../../includes/configure/gpo-settings-2.md)]

---

## User Experience

By controlling the visibility of Settings pages, you can create a customized user experience tailored to your organization's specific needs. Once the policy is applied, users have access only to the Settings pages you explicitly allow, ensuring a focused and streamlined interface.

<!--links-->

[CSP-1]: /windows/client-management/mdm/policy-csp-settings#pagevisibilitylist
[M365-1]: /microsoft-365/admin/misc/organizational-messages-microsoft-365?view=o365-worldwide
[INT-1]: /mem/intune/configuration/settings-catalog
[KB-1]: https://blogs.windows.com/windows-insider/2025/04/25/announcing-windows-11-insider-preview-build-22635-5305-beta-channel/