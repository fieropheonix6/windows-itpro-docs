---
title: Configure Storage Sense in Windows
description: Learn how to configure Storage Sense, a Windows feature that helps manage disk space by automatically cleaning up temporary files and unused content.
ms.topic: how-to
ms.date: 07/10/2025
author: paolomatarazzo
ms.author: paoloma
---

# Configure Storage Sense


Storage Sense is a Windows feature that helps automatically free up disk space by deleting unnecessary files—like temporary files, items in the recycle bin, and previous versions of Windows updates. For IT administrators, especially those managing large device fleets, configuring Storage Sense is a low-effort, high-impact way to ensure devices remain performant and up to date. When left unmanaged, low disk space can prevent critical updates from installing, degrade system performance, and lead to user frustration. By proactively configuring Storage Sense through tools like Microsoft Intune, IT admins can automate storage maintenance and reduce support overhead.

## Practical scenarios

In industries like Education and Frontline Work, devices with limited storage capacity are commonly deployed due to cost and portability considerations. For example, students using 64GB Windows laptops in a 1:1 device program might quickly run out of space due to cached files, downloads, and app data. Similarly, frontline workers using rugged tablets or shared shift-based devices often lack the time or permissions to manage storage manually. In both scenarios, Storage Sense can be configured to automatically clear temporary files and manage OneDrive content, ensuring devices stay responsive and updates install without disruption. This not only improves the user experience but also extends the usable life of the device.

## Configuration

By default, Storage Sense is enabled and configured to run during storage pressure events to automatically reclaim disk space. IT administrators can customize its behavior to align with organizational policies and user requirements.

The available configuration options include:

- Enable or disable Storage Sense at the system level.
- Control cleanup of user temporary files (enable or disable).
- Set retention thresholds for cloud-backed content: define the minimum number of days a file must remain unaccessed before it is offloaded from the local device (while remaining available in the cloud).
- Configure cleanup of the Downloads folder: specify the minimum number of days files must remain unaccessed before deletion.
- Configure cleanup of the Recycle Bin: set the minimum number of days before items are permanently removed.
- Define the execution cadence for Storage Sense: choose from daily, weekly, monthly, or only when disk space is low.

[!INCLUDE [tab-intro](../../../includes/configure/tab-intro.md)]

#### [:::image type="icon" source="../images/icons/intune.svg" border="false"::: **Intune**](#tab/intune)

[!INCLUDE [intune-settings-catalog-1](../../../includes/configure/intune-settings-catalog-1.md)]

| Category | Setting name |
|--|--|
| **Storage** | Allow Storage Sense Global |
| **Storage** | Allow Storage Sense Temporary Files Cleanup |
| **Storage** | Config Storage Sense Cloud Content Dehydration Threshold |
| **Storage** | Config Storage Sense Downloads Cleanup Threshold |
| **Storage** | Config Storage Sense Global Cadence |
| **Storage** | Config Storage Sense Recycle Bin Cleanup Threshold |

[!INCLUDE [intune-settings-catalog-2](../../../includes/configure/intune-settings-catalog-2.md)]

#### [:::image type="icon" source="../images/icons/csp.svg"::: **CSP**](#tab/csp)

You can configure devices using the [Policy CSP][CSP-1].

| Setting |
|--|
|- **OMA-URI:** `./Device/Vendor/MSFT/Policy/Config/Storage/`[AllowStorageSenseGlobal](/windows/client-management/mdm/policy-csp-Storage#allowstoragesenseglobal)<br>- **Data type:** Integer|
|- **OMA-URI:** `./Device/Vendor/MSFT/Policy/Config/Storage/`[AllowStorageSenseTemporaryFilesCleanup](/windows/client-management/mdm/policy-csp-Storage#allowstoragesensetemporaryfilescleanup)<br>- **Data type:** Integer|
|- **OMA-URI:** `./Device/Vendor/MSFT/Policy/Config/Storage/`[ConfigStorageSenseCloudContentDehydrationThreshold](/windows/client-management/mdm/policy-csp-Storage#configstoragesensecloudcontentdehydrationthreshold)<br>- **Data type:** Integer|
|- **OMA-URI:** `./Device/Vendor/MSFT/Policy/Config/Storage/`[ConfigStorageSenseDownloadsCleanupThreshold](/windows/client-management/mdm/policy-csp-Storage#configstoragesensedownloadscleanupthreshold)<br>- **Data type:** Integer|
|- **OMA-URI:** `./Device/Vendor/MSFT/Policy/Config/Storage/`[ConfigStorageSenseGlobalCadence](/windows/client-management/mdm/policy-csp-Storage#configstoragesenseglobalcadence)<br>- **Data type:** Integer|
|- **OMA-URI:** `./Device/Vendor/MSFT/Policy/Config/Storage/`[ConfigStorageSenseRecycleBinCleanupThreshold](/windows/client-management/mdm/policy-csp-Storage#configstoragesenserecyclebincleanupthreshold)<br>- **Data type:** Integer|

#### [:::image type="icon" source="../images/icons/group-policy.svg" border="false"::: **GPO**](#tab/gpo)

[!INCLUDE [gpo-settings-1](../../../includes/configure/gpo-settings-1.md)]

| Group policy path | Group policy setting |
| - | - |
| **Computer Configuration\System\Storage Sense** | Allow Storage Sense |
| **Computer Configuration\System\Storage Sense** | Allow Storage Sense Temporary Files Cleanup |
| **Computer Configuration\System\Storage Sense** | Config Storage Sense Cloud Content Dehydration Threshold |
| **Computer Configuration\System\Storage Sense** | Config Storage Sense Downloads Cleanup Threshold |
| **Computer Configuration\System\Storage Sense** | Config Storage Sense Global Cadence |
| **Computer Configuration\System\Storage Sense** | Config Storage Sense Recycle Bin Cleanup Threshold |

[!INCLUDE [gpo-settings-2](../../../includes/configure/gpo-settings-2.md)]

---

## User Experience

When Storage Sense is configured, users will experience automatic disk space management without needing to manually delete temporary files or manage storage. This leads to a smoother user experience, as devices remain responsive and updates can be installed without issues related to low disk space. Users might notice that temporary files are cleaned up periodically, and they will not have to worry about running out of space due to cached data or old files.

## Related topics

Here are some related topics that can help you learn more about managing disk space in Windows:

- [RestrictLocalStorage](https://learn.microsoft.com/windows/client-management/mdm/sharedpc-csp#restrictlocalstorage)
-

<!--links-->

[CSP-1]: /windows/client-management/mdm/policy-csp-settings#pagevisibilitylist
[M365-1]: /microsoft-365/admin/misc/organizational-messages-microsoft-365?view=o365-worldwide
[INT-1]: /mem/intune/configuration/settings-catalog
