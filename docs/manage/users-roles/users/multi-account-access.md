---
id: multi-account-access
title: Multi-Account Access
description: Multi-account Access allows you to log into multiple Sumo Logic accounts using one username (email address) and password.
---

import useBaseUrl from '@docusaurus/useBaseUrl';

:::note
Sumo Logic recommends subdomains as the best approach to providing access to multiple accounts. You can configure a custom subdomain for each of your Sumo Logic accounts. For more information, see [Set up a custom subdomain](/docs/manage/manage-subscription/create-and-manage-orgs/manage-org-settings/#set-up-a-customsubdomain).
:::

Multi-account access allows you to log into multiple Sumo Logic accounts (also called [organizations](/docs/manage/manage-subscription/create-and-manage-orgs/)) using one username (email address) and password. If the same username already exists in more than one Sumo Logic organization, the accounts are linked automatically. No action is required, though initially, you will be asked to change your password. When you do, this will become your multi-account password.

After you log into Sumo Logic, in the menu next to your name, you will see the list of organizations that you can access. To change organizations, select a new organization from the list.

<img src={useBaseUrl('img/users-roles/multi-account-access-dropdown.png')} alt="Multi-account access menu" style={{border: '1px solid gray'}} width="400" />


## Log in using multi-account access

1. An administrator will add to an organization. When you receive the email welcoming you to the new Sumo Logic organization, log in with your email and password. You will be asked to change your password. This new password will be your multi-account password.
1. In the UI, select a different organization from the list next to your username.
1. You are logged into the second organization.
1. After you log out, when you return, you will be redirected into the organization you were most recently logged into. You can now change between organizations at any time using the same username and password.

### Limitations

* When you log out of Sumo Logic, then log back in again, you will be redirected to the last organization you were logged into. 
* Ensure you save your work before switching to another organization. When you switch to a new organization, you are logged out of your current organization, and any unsaved work is lost. 
* If you belong to two different Sumo Logic organizations, you will count as one allocated user for each organization.
* For multi-account to work, your username (email address) must be the same in the different organizations. If multi-account isn’t working for you, make sure your usernames match in the organizations.
* If you would still like to have an account that is separate from your Sumo Logic multi-account, simply use a different email address. This way, you can keep this account separate from your multi-account username and password.
* Single account users are unaffected by multi-account access, and will not see this option in the UI.

:::note
Your account owner can change the organizations' display name on the Account page. For more information, see [Cloud Flex Legacy Accounts](/docs/manage/manage-subscription/cloud-flex-legacy-accounts) or [Sumo Logic Credits Accounts](/docs/manage/manage-subscription/sumo-logic-credits-accounts), depending on your Sumo Logic packaging.
:::

## Inviting a new user to an organization

When an admin [creates a user](/docs/manage/users-roles/users/create-edit-users/#create-a-user) in a Sumo Logic organization for the first time, the user will receive an email that includes their username and a temporary password.

If that admin creates the same user in a second organization, the user will receive an email that includes their username, and directs them to use their existing (multi-account) password, as shown.

<img src={useBaseUrl('img/users-roles/welcome.png')} alt="Welcome message" style={{border: '1px solid gray'}} width="400" />

## Multi-account and SAML

Multi-account access depends on a username and password to verify your access to multiple accounts. When you log into a session using [SAML](/docs/manage/security/saml), you will not see your multi-account options because Sumo cannot verify your access to those other accounts.

To view multiple orgs with the same email address, you can log out of your SAML session and use your Identity Provider (IdP) to log into the other org.

As an administrator using SAML, if you have users that need to view and switch their account access from one account to another from the Sumo interface, you will need to set them up as whitelisted users on the SAML page and let them log in using a username and password.

## Multi-account, password policies, and Web session timeouts

Sumo Logic Multi-account users may have access to organizations that use different [Password Policies](../../security/set-password-policy.md). With Multi-account, the password policy data from different organizations is centralized.

[**Classic UI**](/docs/get-started/sumo-logic-ui-classic). To access the Password Policy page, in the main Sumo Logic menu select **Administration > Security > Password Policy**. 

[**New UI**](/docs/get-started/sumo-logic-ui/). To access the Password Policy page, in the top menu select **Administration**, and then under **Account Security Settings** select **Password Policy**. You can also click the **Go To...** menu at the top of the screen and select **Password Policy**.
 

In the Password Policy page, you can set the following account settings:

* **Passwords expire in** (days)
* **Password reuse after** (number of unique passwords you must use before you can reuse an old password)
* **Users locked out after** (attempts, time window, and lockout period)

When a user attempts to log in and fails with Multi-account, that user is limited to the most restrictive policies set across those organizations:

* **Passwords expire in** will use the minimum time set across all organizations.
* **Password reuse after** will use the largest number across all organizations.
* **Users locked out after** will take the fewest attempts over the longest time window and then be locked out for the longest period.

Users should be aware that one organization might require only four unique passwords whereas another organization might require 10 unique passwords. As a result, whenever they change their password, they cannot reuse any of the previous 10 passwords.

When you take advantage of Multi-account access and switch to an organization with a shorter session timeout, you will need to re-enter credentials if you have been idle for a period exceeding the session timeout, even if it was much earlier in their session. While it may seem confusing to have different organizations with different timeouts, this is the most secure way to handle this case for Multi-account access. 

## Multi-account and collectors

For Multi-account users, Collector registration with username and password is no longer supported. Multi-account users must use the token or accessid/access key option. 

## Multi-account and APIs

With Multi-account, to use the API, like with Collectors, you will not be able to log in using a username and password. You will be required to use an Access ID and Access Key.

This is due to the fact that Access IDs and Access Keys are tied to an organization, and they are required so that an API request will have the correct context.

API requests made with multiple accounts using only a username and password  will be rejected for insufficient authentication (401).
