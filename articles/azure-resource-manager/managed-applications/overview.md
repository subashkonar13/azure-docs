---
title: Overview of Azure Managed Applications
description: Describes the concepts for Azure Managed Applications that provide cloud solutions that are easy for consumers to deploy and operate.
author: davidsmatlak
ms.author: davidsmatlak
ms.topic: overview
ms.date: 08/03/2022
---

# Azure Managed Applications overview

Azure Managed Applications enable you to offer cloud solutions that are easy for consumers to deploy and operate. You implement the infrastructure and provide ongoing support. To make a managed application available to all customers, publish it in Azure Marketplace. To make it available to only users in your organization, publish it to an internal catalog.

A managed application is similar to a solution template in Azure Marketplace, with one key difference. In a managed application, the resources are deployed to a resource group that's managed by the publisher of the app. The resource group is present in the consumer's subscription, but an identity in the publisher's tenant has access to the resource group. As the publisher, you specify the cost for ongoing support of the solution.

> [!NOTE]
> The documentation for Azure Custom Providers used to be included with Managed Applications. That documentation was moved to [Azure Custom Providers](../custom-providers/overview.md).

## Advantages of managed applications

Managed applications reduce barriers to consumers using your solutions. They don't need expertise in cloud infrastructure to use your solution. Consumers have limited access to the critical resources and don't need to worry about making a mistake when managing it.

Managed applications enable you to establish an ongoing relationship with your consumers. You define terms for managing the application and all charges are handled through Azure billing.

Although customers deploy managed applications in their subscriptions, they don't have to maintain, update, or service them. You can make sure that all customers are using approved versions. Customers don't have to develop application-specific domain knowledge to manage these applications. Customers automatically acquire application updates without the need to worry about troubleshooting and diagnosing issues with the applications.

For IT teams, managed applications enable you to offer pre-approved solutions to users in the organization. You know these solutions are compliant with organizational standards.

Managed Applications support [managed identities for Azure resources](./publish-managed-identity.md).

## Types of managed applications

You can publish your managed application either internally in the service catalog or externally in Azure Marketplace.

:::image type="content" source="./media/overview/manage_app_options.png" alt-text="Diagram that shows how a managed application is published to service catalog or Azure Marketplace.":::

### Service catalog

The service catalog is an internal catalog of approved solutions for users in an organization. You use the catalog to meet organizational standards and offer solutions for the organization. Employees use the catalog to find applications that are recommended and approved by their IT departments. They see the managed applications that other people in their organization share with them.

For information about publishing a managed application to a service catalog, see [Quickstart: Create and publish a managed application definition](publish-service-catalog-app.md).

### Azure Marketplace

Vendors who want to bill for their services can make a managed application available through Azure Marketplace. After the vendor publishes an application, it's available to users outside their organization. With this approach, a managed service provider (MSP), independent software vendor (ISV), or system integrator (SI) can offer their solutions to all Azure customers.

For information about publishing a managed application to Azure Marketplace, see [Create an Azure application offer](../../marketplace/azure-app-offer-setup.md).

## Resource groups for managed applications

Typically, the resources for a managed application are in two resource groups. The consumer manages one resource group, and the publisher manages the other resource group. When the managed application is defined, the publisher specifies the levels of access. The publisher can request either a permanent role assignment, or [just-in-time access](request-just-in-time-access.md) for an assignment that is constrained to a time period.

Restricting access for [data operations](../../role-based-access-control/role-definitions.md) is currently not supported for all data providers in Azure.

The following image shows a scenario where the publisher requests the owner role for the managed resource group. The publisher placed a read-only lock on this resource group for the consumer. The publisher's identities that are granted access to the managed resource group are exempt from the lock.

:::image type="content" source="./media/overview/access.png" alt-text="Diagram that shows access to a managed resource group and application resource group.":::

### Application resource group

This resource group holds the managed application instance. This resource group may only contain one resource. The resource type of the managed application is [Microsoft.Solutions/applications](/azure/templates/microsoft.solutions/applications).

The consumer has full access to the resource group and uses it to manage the lifecycle of the managed application.

### Managed resource group

This resource group holds all the resources that are required by the managed application. For example, this resource group contains the virtual machines, storage accounts, and virtual networks for the solution. The consumer has limited access to this resource group because the consumer doesn't manage the individual resources for the managed application. The publisher's access to this resource group corresponds to the role specified in the managed application definition. For example, the publisher might request the Owner or Contributor role for this resource group. The access is either permanent or limited to a specific time.

When the [managed application is published to the marketplace](../../marketplace/azure-app-offer-setup.md), the publisher can grant consumers the ability to perform specific actions on resources in the managed resource group. For example, the publisher can specify that consumers can restart virtual machines. All other actions beyond read actions are still denied. Changes to resources in a managed resource group by a consumer with granted actions are subject to the [Azure Policy](../../governance/policy/overview.md) assignments within the consumer's tenant scoped to include the managed resource group.

When the consumer deletes the managed application, the managed resource group is also deleted.

## Azure Policy

You can apply an [Azure Policy](../../governance/policy/overview.md) to audit your managed application. You apply policy definitions to make sure deployed instances of your managed application fulfill data and security requirements. If your application interacts with sensitive data, make sure you've evaluated how that should be protected. For example, if your application interacts with data from Microsoft 365, apply a policy definition to make sure data encryption is enabled.

## Next steps

In this article, you learned about benefits of using managed applications. Go to the next article to create a managed application definition.

> [!div class="nextstepaction"]
> [Quickstart: Create and publish a managed application definition](publish-service-catalog-app.md)
