---
title: Create a rule in the Autopilot dashboard in CloudKnox Permissions Management 
description: How to create a rule in the Autopilot dashboard in CloudKnox Permissions Management.
services: active-directory
author: kenwith
manager: rkarlin
ms.service: active-directory
ms.subservice: ciem
ms.workload: identity
ms.topic: how-to
ms.date: 02/23/2022
ms.author: kenwith
---

# Create a rule in the Autopilot dashboard

> [!IMPORTANT]
> CloudKnox Permissions Management (CloudKnox) is currently in PREVIEW.
> Some information relates to a prerelease product that may be substantially modified before it's released. Microsoft makes no warranties, express or implied, with respect to the information provided here.
 
This article describes how to create a rule in the CloudKnox Permissions Management (CloudKnox) **Autopilot** dashboard.

> [!NOTE]
> Only users with **Administrator** permissions can view and make changes on the Autopilot tab. If you don’t have these permissions, contact your system administrator.

## Create a rule 

1. In the CloudKnox home page, select the **Autopilot** tab.
1. In the **Autopilot** dashboard, from the **Authorization system types** dropdown, select Amazon Web Services (**AWS**), Microsoft **Azure**, or Google Cloud Platform (**GCP**).
1. From the **Authorization System** dropdown, in the **List** and **Folders** box, select the account and folder names that you want, and then select **Apply**.  
1. In the **Autopilot** dashboard, select **New Rule**.
1. In the **Rule Name** box, enter a name for your rule.
1. Select **AWS**, **Azure**, **GCP**, and then select **Next**.

1. Select **Authorization Systems**, and then select **All** or the account names that you want.
1. From the **Folders** dropdown, select a folder, and then select **Apply**.

    To change your folder settings, select **Reset**.

    - The **Status** column displays if the authorization system is **Online** or **Offline**.
    - The **Controller** column displays if the controller is **Enabled** or **Not Enabled**.


1. Select **Configure** , and then select the following parameters for your rule:

    - **Role Created On Is**: Select the duration in days.
    - **Role Last Used On Is**: Select the duration in days when the role was last used.
    - **Cross Account Role**: Select **True** or **False**.

1. Select **Mode**, and then, if you want recommendations to be generated and applied manually, select **On-Demand**.
1. Select **Save**

    The following information displays in the **Autopilot Rules** table:

    - **Rule Name**: The name of the rule.
    - **State**: The status of the rule: idle (not being use) or active (being used).
    - **Rule Type**: The type of rule being applied. 
    - **Mode**: The status of the mode: on-demand or not.
    - **Last Generated**: The date and time the rule was last generated.
    - **Created By**: The email address of the user who created the rule.
    - **Last Modified On**: The date and time the rule was last modified.
    - **Subscription**: Provides an **On** or **Off** switch that allows you to receive email notifications when recommendations have been generated, applied, or unapplied.




## Next steps

- For more information about viewing rules, see [View roles in the Autopilot dashboard](cloudknox-ui-autopilot.md).
- For information about generating, viewing, and applying rule recommendations for rules, see [Generate, view, and apply rule recommendations for rules](cloudknox-howto-recommendations-rule.md).
- For information about notification settings for rules, see [View notification settings for a rule](cloudknox-howto-notifications-rule.md).
