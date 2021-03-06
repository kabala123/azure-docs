---
title: Azure Active Directory B2B collaboration FAQs | Microsoft Docs
description: Get answers to frequently asked questions about Azure Active Directory B2B collaboration.

services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: reference
ms.date: 10/29/2018

ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram

ms.collection: M365-identity-device-management
---

# Azure Active Directory B2B collaboration FAQs

These frequently asked questions (FAQs) about Azure Active Directory (Azure AD) business-to-business (B2B) collaboration are periodically updated to include new topics.

### Can we customize our sign-in page so it's more intuitive for our B2B collaboration guest users?
Absolutely! See our [blog post about this feature](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/). For more information about how to customize your organization's sign-in page, see [Add company branding to sign in and Access Panel pages](../fundamentals/customize-branding.md).

### Can B2B collaboration users access SharePoint Online and OneDrive?
Yes. However, the ability to search for existing guest users in SharePoint Online by using the people picker is **Off** by default. To turn on the option to search for existing guest users, set **ShowPeoplePickerSuggestionsForGuestUsers** to **On**. You can turn this setting on either at the tenant level or at the site collection level. You can change this setting by using the Set-SPOTenant and Set-SPOSite cmdlets. With these cmdlets, members can search all existing guest users in the directory. Changes in the tenant scope don't affect SharePoint Online sites that have already been provisioned.

### Is the CSV upload feature still supported?
Yes. For more information about using the .csv file upload feature, see [this PowerShell sample](code-samples.md).

### How can I customize my invitation emails?
You can customize almost everything about the inviter process by using the [B2B invitation APIs](customize-invitation-api.md).

### Can guest users reset their multi-factor authentication method?
Yes. Guest users can reset their multi-factor authentication method the same way that regular users do.

### Which organization is responsible for multi-factor authentication licenses?
The inviting organization performs multi-factor authentication. The inviting organization must make sure that the organization has enough licenses for their B2B users who are using multi-factor authentication.

### What if a partner organization already has multi-factor authentication set up? Can we trust their multi-factor authentication, and not use our own multi-factor authentication?
This feature is planned for a future release, so that then you can select specific partners to exclude from your (the inviting organization's) multi-factor authentication.

### How can I use delayed invitations?
An organization might want to add B2B collaboration users, provision them to applications as needed, and then send invitations. You can use the B2B collaboration invitation API to customize the onboarding workflow.

### Can I make guest users visible in the Exchange Global Address List?
Yes. Guest objects aren't visible in your organization's global address list by default, but you can use Azure Active Directory PowerShell to make them visible. See **Can I make guest objects visible in the global address list?** in [Guest access in Office 365 Groups](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6#PickTab=FAQ).

### Can I make a guest user a limited administrator?
Absolutely. For more information, see [Adding guest users to a role](add-guest-to-role.md).

### Does Azure AD B2B collaboration allow B2B users to access the Azure portal?
Unless a user is assigned the role of limited administrator or global administrator, B2B collaboration users won't require access to the Azure portal. However, B2B collaboration users who are assigned the role of limited administrator or global administrator can access the portal. Also, if a guest user who isn't assigned one of these admin roles accesses the portal, the user might be able to access certain parts of the experience. The guest user role has some permissions in the directory.

### Can I block access to the Azure portal for guest users?
Yes! When you configure this policy, be careful to avoid accidentally blocking access to members and admins.
To block a guest user's access to the [Azure portal](https://portal.azure.com), use a conditional access policy in the Windows Azure classic deployment model API:
1. Modify the **All Users** group so that it contains only members.
  ![modify the group screenshot](media/faq/modify-all-users-group.png)
2. Create a dynamic group that contains guest users.
  ![create group screenshot](media/faq/group-with-guest-users.png)
3. Set up a conditional access policy to block guest users from accessing the portal, as shown in the following video:
  
  > [!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### Does Azure AD B2B collaboration support multi-factor authentication and consumer email accounts?
Yes. Multi-factor authentication and consumer email accounts are both supported for Azure AD B2B collaboration.

### Do you support password reset for Azure AD B2B collaboration users?
If your Azure AD tenant is the home directory for a user, you can [reset the user's password](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-reset-password-azure-portal) from the Azure portal. But you can't directly reset a password for a guest user who signs in with an account that's managed by another Azure AD directory or external identity provider. Only the guest user or an administrator in the user’s home directory can reset the password. Here are some examples of how password reset works for guest users:
 
* Guest users who sign in with a Microsoft account (for example guestuser@live.com) can reset their own passwords using Microsoft account self-service password reset (SSPR). See [How to reset your Microsoft account password](https://support.microsoft.com/help/4026971/microsoft-account-how-to-reset-your-password).
* Guest users who sign in with a Google account or another external identity provider can reset their own passwords using their identity provider’s SSPR method. For example, a guest user with the Google account guestuser@gmail.com can reset their password by following the instructions in [Change or reset your password](https://support.google.com/accounts/answer/41078).
* If the identity tenant is a just-in-time (JIT) or "viral" tenant (meaning it's a separate, unmanaged Azure tenant), only the guest user can reset their password. Sometimes an organization will [take over management of viral tenants](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover) that are created when employees use their work email addresses to sign up for services. After the organization takes over a viral tenant, only an administrator in that organization can reset the user's password or enable SSPR. If necessary, as the inviting organization, you can remove the guest user account from your directory and resend an invitation.
* If the guest user's home directory is your Azure AD tenant, you can reset the user's password. For example, you might have created a user or synced a user from your on-premises Active Directory and set their UserType to Guest. Because this user is homed in your directory, you can reset their password from the Azure portal.

### Does Microsoft Dynamics 365 provide online support for Azure AD B2B collaboration?
Yes, Dynamics 365 (online) supports Azure AD B2B collaboration. For more information, see the Dynamics 365 article [Invite users with Azure AD B2B collaboration](https://docs.microsoft.com/dynamics365/customer-engagement/admin/invite-users-azure-active-directory-b2b-collaboration).

### What is the lifetime of an initial password for a newly created B2B collaboration user?
Azure AD has a fixed set of character, password strength, and account lockout requirements that apply equally to all Azure AD cloud user accounts. Cloud user accounts are accounts that aren't federated with another identity provider, such as 
* Microsoft account
* Facebook
* Active Directory Federation Services
* Another cloud tenant (for B2B collaboration)

For federated accounts, password policy depends on the policy that is applied in the on-premises tenancy and the user's Microsoft account settings.

### An organization might want to have different experiences in their applications for tenant users and guest users. Is there standard guidance for this? Is the presence of the identity provider claim the correct model to use?
A guest user can use any identity provider to authenticate. For more information, see [Properties of a B2B collaboration user](user-properties.md). Use the **UserType** property to determine user experience. The **UserType** claim isn't currently included in the token. Applications should use the Graph API to query the directory for the user, and to get the UserType.

### Where can I find a B2B collaboration community to share solutions and to submit ideas?
We're constantly listening to your feedback, to improve B2B collaboration. Please share your user scenarios, best practices, and what you like about Azure AD B2B collaboration. Join the discussion in the [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
 
We also invite you to submit your ideas and vote for future features at [B2B Collaboration Ideas](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas).

### Can we send an invitation that is automatically redeemed, so that the user is just “ready to go”? Or does the user always have to click through to the redemption URL?
You can invite other users in the partner organization by using the UI, PowerShell scripts, or APIs. You can then send the guest user a direct link to a shared app. In most cases, there's no longer a need to open the email invitation and click a redemption URL. See [Azure Active Directory B2B collaboration invitation redemption](redemption-experience.md).

### How does B2B collaboration work when the invited partner is using federation to add their own on-premises authentication?
If the partner has an Azure AD tenant that is federated to the on-premises authentication infrastructure, on-premises single sign-on (SSO) is automatically achieved. If the partner doesn't have an Azure AD tenant, an Azure AD account is created for new users. 

### I thought Azure AD B2B didn't accept gmail.com and outlook.com email addresses, and that B2C was used for those kinds of accounts?
We are removing the differences between B2B and business-to-consumer (B2C) collaboration in terms of which identities are supported. The identity used isn't a good reason to choose between using B2B or using B2C. For information about choosing your collaboration option, see [Compare B2B collaboration and B2C in Azure Active Directory](compare-with-b2c.md).

### What applications and services support Azure B2B guest users?
All Azure AD-integrated applications can support Azure B2B guest users, but they must use a tenanted endpoint to authenticate guest users. You might also need to [customize the claims](claims-mapping.md) in the SAML token that is issued when a guest user authenticates to the app. 

### Can we force multi-factor authentication for B2B guest users if our partners don't have multi-factor authentication?
Yes. For more information, see [Conditional access for B2B collaboration users](conditional-access.md).

### In SharePoint, you can define an "allow" or "deny" list for external users. Can we do this in Azure?
Yes. Azure AD B2B collaboration supports allow lists and deny lists. 

### What licenses do we need to use Azure AD B2B?
For information about what licenses your organization needs to use Azure AD B2B, see [Azure Active Directory B2B collaboration licensing guidance](licensing-guidance.md).

### Next steps

- [What is Azure AD B2B collaboration?](what-is-b2b.md)

