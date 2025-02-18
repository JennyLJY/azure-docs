---
title: Enabling Data Use Management on your Microsoft Purview sources
description: Step-by-step guide on how to enable data use access for your registered sources.
author: inward-eye
ms.author: vlrodrig
ms.service: purview
ms.subservice: purview-data-policies
ms.topic: how-to
ms.date: 8/10/2022
ms.custom:
---

# Enable Data Use Management on your Microsoft Purview sources

[!INCLUDE [feature-in-preview](includes/feature-in-preview.md)]

*Data Use Management* (DUM) is an option within the data source registration in Microsoft Purview. This option lets Microsoft Purview manage data access for your resources. The high level concept is that the data owner allows its data resource to be available for access policies by enabling *DUM*. 

Currently, a data owner can enable DUM on a data resource for these types of access policies:

* [Data owner access policies](concept-policies-data-owner.md) - access policies authored via Microsoft Purview data policy experience.
* [Self-service access policies](concept-self-service-data-access-policy.md) - access policies automatically generated by Microsoft Purview after a [self-service access request](how-to-request-access.md) is approved.

To be able to create any data policy on a resource, DUM must first be enabled on that resource. This article will explain how to enable DUM on your resources in Microsoft Purview.

>[!IMPORTANT]
>Because Data Use Management directly affects access to your data, it directly affects your data security. Review [**additional considerations**](#additional-considerations-related-to-data-use-management) and [**best practices**](#data-use-management-best-practices) below before enabling DUM in your environment.

## Prerequisites
[!INCLUDE [Access policies generic configuration](./includes/access-policies-configuration-generic.md)]

## Enable Data Use Management

To enable *Data Use Management* for a resource, the resource will first need to be registered in Microsoft Purview.
To register a resource, follow the **Prerequisites** and **Register** sections of the [source pages](azure-purview-connector-overview.md) for your resources.

Once you have your resource registered, follow the rest of the steps to enable an individual resource for *Data Use Management*.

1. Go to the [Microsoft Purview governance portal](https://web.purview.azure.com/resource/).

1. Select the **Data map** tab in the left menu.

1. Select the **Sources** tab in the left menu.

1. Select the source where you want to enable *Data Use Management*.

1. At the top of the source page, select **Edit source**.

1. Set the *Data Use Management* toggle to **Enabled**, as shown in the image below.

:::image type="content" source="./media/tutorial-data-owner-policies-storage/register-data-source-for-policy-storage.png" alt-text="Set Data Use Management toggle to **Enabled** at the bottom of the menu.":::

## Disable Data Use Management

To disable Data Use Management for a source, resource group, or subscription, a user needs to either be a resource IAM **Owner** or a Microsoft Purview **Data source admin**. Once you have those permissions follow these steps:

1. Go to the [Microsoft Purview governance portal](https://web.purview.azure.com/resource/).

1. Select the **Data map** tab in the left menu.

1. Select the **Sources** tab in the left menu.

1. Select the source you want to disable Data Use Management for.

1. At the top of the source page, select **Edit source**.

1. Set the **Data Use Management** toggle to **Disabled**.

## Additional considerations related to Data Use Management
- Make sure you write down the **Name** you use when registering in Microsoft Purview. You will need it when you publish a policy. The recommended practice is to make the registered name exactly the same as the endpoint name.
- To disable a source for *Data Use Management*, remove it first from being bound (i.e. published) in any policy.
- While user needs to have both data source *Owner* and Microsoft Purview *Data source admin* to enable a source for *Data Use Management*, either of those roles can independently disable it.
- Disabling *Data Use Management* for a subscription will disable it also for all assets registered in that subscription.

> [!WARNING]
> **Known issues** related to source registration
> - Moving data sources to a different resource group or subscription is not supported. If want to do that, de-register the data source in Microsoft Purview before moving it and then register it again after that happens. Note that policies are bound to the data source ARM path. Changing the data source subscription or resource group makes policies ineffective.
> - Once a subscription gets disabled for *Data Use Management* any underlying assets that are enabled for *Data Use Management* will be disabled, which is the right behavior. However, policy statements based on those assets will still be allowed after that.

## Data Use Management best practices
- We highly encourage registering data sources for *Data Use Management* and managing all associated access policies in a single Microsoft Purview account.
- Should you have multiple Microsoft Purview accounts, be aware that **all** data sources belonging to a subscription must be registered for *Data Use Management* in a single Microsoft Purview account. That Microsoft Purview account can be in any subscription in the tenant. The *Data Use Management* toggle will become greyed out when there are invalid configurations. Some examples of valid and invalid configurations follow in the diagram below:
    - **Case 1** shows a valid configuration where a Storage account is registered in a Microsoft Purview account in the same subscription.
    - **Case 2** shows a valid configuration where a Storage account is registered in a Microsoft Purview account in a different subscription. 
    - **Case 3** shows an invalid configuration arising because Storage accounts S3SA1 and S3SA2 both belong to Subscription 3, but are registered to different Microsoft Purview accounts. In that case, the *Data Use Management* toggle will only enable in the Microsoft Purview account that wins and registers a data source in that subscription first. The toggle will then be greyed out for the other data source.
- If the *Data Use Management* toggle is greyed out and cannot be enabled, hover over it to know the name of the Microsoft Purview account that has registered the data resource first.

![Diagram shows valid and invalid configurations when using multiple Microsoft Purview accounts to manage policies.](./media/access-policies-common/valid-and-invalid-configurations.png)

## Next steps

- [Create data owner policies for your resources](how-to-policies-data-owner-authoring-generic.md)
- [Enable Microsoft Purview data owner policies on all data sources in a subscription or a resource group](./how-to-policies-data-owner-resource-group.md)
- [Enable Microsoft Purview data owner policies on an Azure Storage account](./how-to-policies-data-owner-storage.md)
