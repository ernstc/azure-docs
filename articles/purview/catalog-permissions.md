---
title: Understand access and permissions in the Microsoft Purview Data Map
description: This article gives an overview permission, access control, and collections in the Microsoft Purview Data Map. Role-based access control is managed within the Microsoft Purview Data Map itself, so this guide will cover the basics to secure your information.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.custom: event-tier1-build-2022
ms.topic: conceptual
ms.date: 05/16/2022
---

# Access control in the Microsoft Purview Data Map

The Microsoft Purview Data Map uses **Collections** to organize and manage access across its sources, assets, and other artifacts. This article describes collections and access management in your Microsoft Purview Data Map.

> [!IMPORTANT]
> This article refers to permissions required for the Microsoft Purview governance portal, and applications like the Microsoft Purview Data Map, Data Catalog, Data Estate Insights, etc. If you are looking for permissions information for the Microsoft Purview compliance center, follow [the article for permissions in the Microsoft Purview compliance portal](/microsoft-365/compliance/microsoft-365-compliance-center-permissions).

## Collections

A collection is a tool Microsoft Purview uses to group assets, sources, and other artifacts into a hierarchy for discoverability and to manage access control. All accesses to Microsoft Purview's resources are managed from collections in the Microsoft Purview account itself.

## Roles

Microsoft Purview uses a set of predefined roles to control who can access what within the account. These roles are currently:

- **Collection administrator** - a role for users that will need to assign roles to other users in Microsoft Purview or manage collections. Collection admins can add users to roles on collections where they're admins. They can also edit collections, their details, and add subcollections.
- **Data curators** - a role that provides access to the data catalog to manage assets, configure custom classifications, set up glossary terms, and view data estate insights. Data curators can create, read, modify, move, and delete assets. They can also apply annotations to assets.
- **Data readers** - a role that provides read-only access to data assets, classifications, classification rules, collections and glossary terms.
- **Data source administrator** - a role that allows a user to manage data sources and scans. If a user is granted only to **Data source admin** role on a given data source, they can run new scans using an existing scan rule. To create new scan rules, the user must be also granted as either **Data reader** or **Data curator** roles.
- **Insights reader** - a role that provides read-only access to insights reports for collections where the insights reader also has at least the **Data reader** role. For more information, see [insights permissions.](insights-permissions.md)
- **Policy author (Preview)** - a role that allows a user to view, update, and delete Microsoft Purview policies through the policy management app within Microsoft Purview.
- **Workflow administrator** - a role that allows a user to access the workflow authoring page in the Microsoft Purview governance portal, and publish workflows on collections where they have access permissions. Workflow administrator only has access to authoring, and so will need at least Data reader permission on a collection to be able to access the Purview governance portal.

> [!NOTE] 
> At this time, Microsoft Purview Policy author role is not sufficient to create policies. The Microsoft Purview Data source admin role is also required.

## Who should be assigned to what role?

|User Scenario|Appropriate Role(s)|
|-------------|-----------------|
|I just need to find assets, I don't want to edit anything|Data reader|
|I need to edit information about assets, assign classifications, associate them with glossary entries, and so on.|Data curator|
|I need to edit the glossary or set up new classification definitions|Data curator|
|I need to view Data Estate Insights to understand the governance posture of my data estate|Data curator|
|My application's Service Principal needs to push data to Microsoft Purview|Data curator|
|I need to set up scans via the Microsoft Purview governance portal|Data curator on the collection **or** data curator **and** data source administrator where the source is registered.|
|I need to enable a Service Principal or group to set up and monitor scans in Microsoft Purview without allowing them to access the catalog's information |Data source administrator|
|I need to put users into roles in Microsoft Purview | Collection administrator |
|I need to create and publish access policies | Data source administrator and policy author |
|I need to create workflows for my Microsoft Purview account | Workflow administrator |
|I need to view insights for collections I'm a part of | Insights reader **or** data curator |

:::image type="content" source="media/catalog-permissions/catalog-permission-role.svg" alt-text="Chart showing Microsoft Purview roles" lightbox="media/catalog-permissions/catalog-permission-role.svg":::
>[!NOTE]
> **\*Data source administrator permissions on Policies** - Data source administrators are also able to publish data policies.

## Understand how to use Microsoft Purview's roles and collections

All access control is managed in Microsoft Purview's collections. Microsoft Purview's collections can be found in the [Microsoft Purview governance portal](https://web.purview.azure.com/resource/). Open your Microsoft Purview account in the [Azure portal](https://portal.azure.com) and select the Microsoft Purview governance portal tile on the Overview page. From there, navigate to the data map on the left menu, and then select the 'Collections' tab.

When a Microsoft Purview account is created, it starts with a root collection that has the same name as the Microsoft Purview account itself. The creator of the Microsoft Purview account is automatically added as a Collection Admin, Data Source Admin, Data Curator, and Data Reader on this root collection, and can edit and manage this collection.

Sources, assets, and objects can be added directly to this root collection, but so can other collections. Adding collections will give you more control over who has access to data across your Microsoft Purview account.

All other users can only access information within the Microsoft Purview account if they, or a group they're in, are given one of the above roles. This means, when you create a Microsoft Purview account, no one but the creator can access or use its APIs until they're [added to one or more of the above roles in a collection](how-to-create-and-manage-collections.md#add-role-assignments).

Users can only be added to a collection by a collection admin, or through permissions inheritance. The permissions of a parent collection are automatically inherited by its subcollections. However, you can choose to [restrict permission inheritance](how-to-create-and-manage-collections.md#restrict-inheritance) on any collection. If you do this, its subcollections will no longer inherit permissions from the parent and will need to be added directly, though collection admins that are automatically inherited from a parent collection can't be removed.

You can assign Microsoft Purview roles to users, security groups and service principals from your Azure Active Directory that is associated with your purview account's subscription.

## Assign permissions to your users

After creating a Microsoft Purview account, the first thing to do is create collections and assign users to roles within those collections.

> [!NOTE]
> If you created your Microsoft Purview account using a service principal, to be able to access the Microsoft Purview governance portal and assign permissions to users, you will need to grant a user collection admin permissions on the root collection.
> You can use [this Azure CLI command](/cli/azure/purview/account#az-purview-account-add-root-collection-admin):
>
>   ```azurecli
>   az purview account add-root-collection-admin --account-name [Microsoft Purview Account Name] --resource-group [Resource Group Name] --object-id [User Object Id]
>   ```
> The object-id is optional. For more information and an example, see the [CLI command reference page](/cli/azure/purview/account#az-purview-account-add-root-collection-admin).

### Create collections

Collections can be customized for structure of the sources in your Microsoft Purview account, and can act like organized storage bins for these resources. When you're thinking about the collections you might need, consider how your users will access or discover information. Are your sources broken up by departments? Are there specialized groups within those departments that will only need to discover some assets? Are there some sources that should be discoverable by all your users?

This will inform the collections and subcollections you may need to most effectively organize your data map.

New collections can be added directly to the data map where you can choose their parent collection from a drop-down, or they can be added from the parent as a sub collection. In the data map view, you can see all your sources and assets ordered by the collections, and in the list, the source's collection is listed.

For more instructions and information, you can follow our [guide for creating and managing collections](how-to-create-and-manage-collections.md).

#### A collections example

Now that we have a base understanding of collections, permissions, and how they work, let's look at an example.

:::image type="content" source="./media/catalog-permissions/collection-example.png" alt-text="Chart showing a sample collections hierarchy broken up by region and department." border="true":::

This is one way an organization might structure their data: Starting with their root collection (Contoso, in this example) collections are organized into regions, and then into departments and subdepartments. Data sources and assets can be added to any one these collections to organize data resources by these regions and department, and manage access control along those lines. There's one subdepartment, Revenue, that has strict access guidelines, so permissions will need to be tightly managed.

The [data reader role](#roles) can access information within the catalog, but not manage or edit it. So for our example above, adding the Data Reader permission to a group on the root collection and allowing inheritance will give all users in that group reader permissions on Microsoft Purview sources and assets. This makes these resources discoverable, but not editable, by everyone in that group. [Restricting inheritance](how-to-create-and-manage-collections.md#restrict-inheritance) on the Revenue group will control access to those assets. Users who need access to revenue information can be added separately to the Revenue collection.
Similarly with the Data Curator and Data Source Admin roles, permissions for those groups will start at the collection where they're assigned and trickle down to subcollections that haven't restricted inheritance. Below we have assigned permissions for several groups at collections levels in the Americas sub collection.

:::image type="content" source="./media/catalog-permissions/collection-permissions-example.png" alt-text="Chart showing a sample collections hierarchy broken up by region and department showing permissions distribution." border="true":::

### Add users to roles

Role assignment is managed through the collections. Only a user with the [collection admin role](#roles) can grant permissions to other users on that collection. When new permissions need to be added, a collection admin will access the [Microsoft Purview governance portal](https://web.purview.azure.com/resource/), navigate to data map, then the collections tab, and select the collection where a user needs to be added. From the Role Assignments tab, they'll be able to add and manage users who need permissions.

For full instructions, see our [how-to guide for adding role assignments](how-to-create-and-manage-collections.md#add-role-assignments).

## Administrator change

There may be a time when your [root collection admin](#roles) needs to change. By default, the user who creates the Microsoft Purview account is automatically assigned collection admin to the root collection. To update the root collection admin, there are three options:

- You can [assign permissions through the portal](how-to-create-and-manage-collections.md#add-role-assignments) as you have for any other role.

- You can use the REST API to add a collection administrator. Instructions to use the REST API to add a collection admin can be found in our [REST API for collections documentation.](tutorial-metadata-policy-collections-apis.md#add-the-root-collection-administrator-role) For additional information, you can see our [REST API reference](/rest/api/purview/accounts/add-root-collection-admin).

- You can also use [the below Azure CLI command](/cli/azure/purview/account#az-purview-account-add-root-collection-admin). The object-id is optional. For more information and an example, see the [CLI command reference page](/cli/azure/purview/account#az-purview-account-add-root-collection-admin).

    ```azurecli
    az purview account add-root-collection-admin --account-name [Microsoft Purview Account Name] --resource-group [Resource Group Name] --object-id [User Object Id]
    ```

## Next steps

Now that you have a base understanding of collections, and access control, follow the guides below to create and manage those collections, or get started with registering sources into your Microsoft Purview Resource.

- [How to create and manage collections](how-to-create-and-manage-collections.md)
- [Microsoft Purview supported data sources](azure-purview-connector-overview.md)
