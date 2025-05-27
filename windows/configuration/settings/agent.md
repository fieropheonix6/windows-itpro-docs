---
title: Configure the agent in Windows Settings
description: Learn how to configure the agent in Windows Settings.
ms.topic: how-to
ms.date: 05/05/2025
author: paolomatarazzo
ms.author: paoloma
appliesto:
  - "✅ <a href=\"https://learn.microsoft.com/windows-insider/flight-hub\" target=\"_blank\">Windows Insider (Beta Channel)</a>"
---

# Configure the agent in Windows Settings

[!INCLUDE [insider-feature](../includes/insider-feature.md)]

Starting with [Windows Insider 22635.xxxx (Beta Channel)][KB-1], the agent in Windows Settings is a feature that uses on-device AI to help you find and change settings on your PC. It can also help you troubleshoot issues by providing recommendations and automating tasks based on your input.

The agent guides users through finding and changing settings, offering relevant information and recommendations. With user permission, it can automate the process of changing settings. This feature enhances search within Windows Settings by enabling natural language queries and utilizing an AI model for intelligent search suggestions.

The agent is designed to respect existing policy settings on the device, ensuring that it doesn't override any restrictions already in place.

:::image type="content" source="images/settings-agent.png" alt-text="Screenshot of Settings showing the search agent." border="false":::


## Requirements

The agent in Settings is currently available only on Windows 11 devices that meet the following criteria:

### System requirements

> [!div class="checklist"]
> - Windows Insider 22635.xxxx (Beta Channel) and later
> - A [Copilot+ PC](https://aka.ms/copilotpluspcs)
> - Qualcomm Snapdragon. Intel and AMD support will be available at a later time
> - The device must have enabled the [temporary enterprise feature control](/windows/whats-new/temporary-enterprise-feature-control) policy setting

> [!NOTE]
> Settings agent is not available on Windows IoT devices.

### Language requirements

> [!div class="checklist"]
> - English

### Geography requirements

> [!div class="checklist"]
> - All countries, except for China and Canada

## How it works

The agent in Settings uses a lightweight language model called *Settings Mu*, which is fine-tuned using Settings data to help users quickly find and adjust system settings.

The model runs locally on the device, analyzing a user's query to match with relevant settings already available in Settings. If the model can't confidently classify a query to a specific setting, standard search results are displayed instead.

> [!NOTE]
> The agent only suggests settings, and it doesn't make any changes automatically. A user must select the **Apply** button to enable or disable a setting. If needed, a user can easily *undo* the change.

The Settings Mu model has undergone fairness evaluations, as well as comprehensive Responsible AI, security, and privacy assessments. These steps ensure the technology is effective, equitable, and aligned with [Microsoft's Responsible AI principles](https://www.microsoft.com/ai/responsible-ai).

## Configure the agent

As an administrator, you can enable or disable the agent in Settings using a policy setting:

- When the policy setting is enabled, the agent experience isn't available, and search results are limited to statically indexed searches and semantic searches.
- When the policy setting is disabled (default), the agent search experience is available. The agent can provide recommendations and automate tasks based on user input.


## Configuration

[!INCLUDE [tab-intro](../../../includes/configure/tab-intro.md)]

#### [:::image type="icon" source="../images/icons/intune.svg" border="false"::: **Intune**](#tab/intune)

[!INCLUDE [intune-settings-catalog-1](../../../includes/configure/intune-settings-catalog-1.md)]

| Category | Setting name |
|--|--|
| **Windows AI** | Disable Settings Agent |

[!INCLUDE [intune-settings-catalog-2](../../../includes/configure/intune-settings-catalog-2.md)]

#### [:::image type="icon" source="../images/icons/csp.svg"::: **CSP**](#tab/csp)

You can configure devices using the [Policy CSP][CSP-1].

| Setting |
|--|
|- **OMA-URI:** `./Vendor/MSFT/Policy/Config/WindowsAI/`[DisableSettingsAgent](/windows/client-management/mdm/policy-csp-windowsai)<br>- **Data type:** Boolean<br>- **Value:** <br>&nbsp;&nbsp;&nbsp;&nbsp;- `0` (default): the agent search experience in Settings is enabled <br>&nbsp;&nbsp;&nbsp;&nbsp;- `1`: the agent search experience in Settings is disabled|

#### [:::image type="icon" source="../images/icons/group-policy.svg" border="false"::: **GPO**](#tab/gpo)

[!INCLUDE [gpo-settings-1](../../../includes/configure/gpo-settings-1.md)]

| Group policy path | Group policy setting |
| - | - |
| **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Windows AI** | Disable Settings Agent |

[!INCLUDE [gpo-settings-2](../../../includes/configure/gpo-settings-2.md)]

---

## User experience

If the agent is enabled, users can access it by opening Settings and typing their question in the search box. The agent will provide relevant information and recommendations based on the user's input.

:::image type="content" source="images/settings-agent-example.png" alt-text="Screenshot of Settings showing the search agent with an example search." border="false":::

<!--links-->

[CSP-1]: /windows/client-management/mdm/policy-csp-windowsai
[M365-1]: /microsoft-365/admin/misc/organizational-messages-microsoft-365?view=o365-worldwide
[INT-1]: /mem/intune/configuration/settings-catalog
[KB-1]: https://blogs.windows.com/windows-insider/2025/04/25/announcing-windows-11-insider-preview-build-22635-5305-beta-channel/