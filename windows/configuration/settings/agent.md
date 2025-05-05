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

:::image type="content" source="images/settings-agent.png" alt-text="Screenshot of Settings showing the search agent." border="false":::

> [!NOTE]
> The Settings agent respects the policy settings already configured on the device. For example, if a user is restricted from accessing certain settings, the agent won't be able to change those settings on their behalf.

## System requirements

Here's a list of requirements to use the Settings agent:

> [!div class="checklist"]
> - Windows Insider 22635.xxxx (Beta Channel) and later
> - A [Copilot+ PC](https://aka.ms/copilotpluspcs)

> [!NOTE]
> Settings agent is not available on Windows IoT devices.

## Settings agent policy setting

As an administrator, you can control the Settings agent's visibility using policy settings.

The Settings agent experience enhances search within Windows Settings by enabling natural language. When activated, it utilizes an AI model to provide intelligent Settings search suggestions. The policy setting allows you to determine whether the Settings agent search experience is available for users on their devices.

- When the policy setting is enabled, the agent experience isn't available, and search results are limited to statically indexed searches and semantic searches.
- When the policy setting is disabled (default), the Settings agent search experience is available, and the agent can provide recommendations and automate tasks based on user input.


## Configuration

[!INCLUDE [tab-intro](../../../includes/configure/tab-intro.md)]

#### [:::image type="icon" source="../images/icons/intune.svg" border="false"::: **Intune**](#tab/intune)

[!INCLUDE [intune-settings-catalog-1](../../../includes/configure/intune-settings-catalog-1.md)]

| Category | Setting name | Value |
|--|--|--|
| **Windows AI** | - Disable Settings Agent | Toggle to enable or disable the Settings agent search experience |

[!INCLUDE [intune-settings-catalog-2](../../../includes/configure/intune-settings-catalog-2.md)]

#### [:::image type="icon" source="../images/icons/csp.svg"::: **CSP**](#tab/csp)

You can configure devices using the [Policy CSP][CSP-1].

| Setting |
|--|
|- **OMA-URI:** `./Vendor/MSFT/Policy/Config/WindowsAI/DisableSettingsAgent`<br>- **Data type:** Boolean<br>- **Value:** <br> - `0` (default): Settings agent search experience is enabled <br>- `1`: Settings agent search experience is disabled|

#### [:::image type="icon" source="../images/icons/group-policy.svg" border="false"::: **GPO**](#tab/gpo)

[!INCLUDE [gpo-settings-1](../../../includes/configure/gpo-settings-1.md)]

| Group policy path | Group policy setting | Value |
| - | - | - |
| **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Windows AI** | Disable Settings Agent | |

[!INCLUDE [gpo-settings-2](../../../includes/configure/gpo-settings-2.md)]

---

<!--links-->

[CSP-1]: /windows/client-management/mdm/policy-csp-settings#pagevisibilitylist
[M365-1]: /microsoft-365/admin/misc/organizational-messages-microsoft-365?view=o365-worldwide
[INT-1]: /mem/intune/configuration/settings-catalog
[KB-1]: https://blogs.windows.com/windows-insider/2025/04/25/announcing-windows-11-insider-preview-build-22635-5305-beta-channel/