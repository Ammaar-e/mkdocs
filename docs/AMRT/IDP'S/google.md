# Google IDP
We use google as our IDP for staff and students to access Google Products. The following information below is how to set it up. The account connected to Google Worspace ad the admin is interservices@amrt.xyz

# Lets Set it up
### This document shows you how to set up user provisioning and single sign-on between a Microsoft Azure AD tenant and your Cloud Identity or Google Workspace account. The document assumes that you already use Microsoft Office 365 or Azure AD in your organization and want to use Azure AD for allowing users to authenticate with Google Cloud. Azure AD itself might be connected to an on-premises Active Directory and might use AD FS federation, pass-through authentication, or password hash synchronization.

!!! info "Note: fees"

    This article uses classic organizational SSO profiles to set up single sign-on. Using SAML profiles is currently incompatible with some Azure AD features, including Azure AD B2B.
    Objectives
    Set up Azure AD to automatically provision users and, optionally, groups to Cloud Identity or Google Workspace.
    Configure single sign-on to allow users to sign in to Google Cloud by using an Azure AD user account or a user that has been provisioned from Active Directory to Azure AD.
    Costs
    If you are using the free edition of Cloud Identity, setting up federation with Azure AD won't use any billable components of Google Cloud.
    Check the Azure AD pricing page for any fees that might apply to using Azure AD.

## Before you begin
Make sure you understand the differences between connecting Google Cloud to Azure AD versus directly connecting Google Cloud to Active Directory.
Decide how you want to map identities, groups, and domains between Azure AD and Cloud Identity or Google Workspace. Specifically, answer the following questions:
Do you plan to use email addresses or User Principal Names (UPNs) as common identifiers for users?
Do you plan to provision groups? If so, do you plan to map groups by email address or by name?
Do you plan to provision all users to Google Cloud or only a select subset of users?
Before connecting your production Azure AD tenant to Google Cloud, consider using an Azure AD test tenant for setting up and testing user provisioning.
Sign up for Cloud Identity if you don't have an account already.
If you're using the free edition of Cloud Identity and intend to provision more than 50 users, request an increase of the total number of free Cloud Identity users through your support contact.
If you suspect that any of the domains you plan to use for Cloud Identity could have been used by employees to register consumer accounts, consider migrating these user accounts first. For more details, see Assessing existing user accounts.
Note: This document refers to the Google Cloud/G Suite Connector by Microsoft gallery app from the Microsoft Azure marketplace. This app is a Microsoft product and is not maintained or supported by Google.
Preparing your Cloud Identity or Google Workspace account
Create a user for Azure AD
To let Azure AD access your Cloud Identity or Google Workspace account, you must create a user for Azure AD in your Cloud Identity or Google Workspace account.

The Azure AD user is only intended for automated provisioning. Therefore, it's best to keep it separate from other user accounts by placing it in a separate organizational unit (OU). Using a separate OU also ensures that you can later disable single sign-on for the Azure AD user.

To create a new OU, do the following:

Open the Admin Console and log in using the super-admin user created when you signed up for Cloud Identity or Google Workspace.
In the menu, go to Directory > Organizational units.
Click Create organizational unit and provide an name and description for the OU:
Name: Automation
Description: Automation users
Click Create.
Create a user account for Azure AD and place it in the Automation OU:

In the menu, go to Directory > Users and click Add new user to create a user.
Provide an appropriate name and email address such as the following:

First Name: Azure AD
Last Name: Provisioning
Primary email: azuread-provisioning

Keep the primary domain for the email address.

Click Manage user's password, organizational unit, and profile photo and configure the following settings:

Organizational unit: Select the Automation OU that you created previously.
Password: Select Create password and enter a password.
Ask for a password change at the next sign-in: Disabled.
Click Add new user.

Click Done.

Assign privileges to Azure AD
To let Azure AD create, list, and suspend users and groups in your Cloud Identity or Google Workspace account, you must grant the azuread-provisioning user additional privileges as follows:

To allow Azure AD to manage all users, including delegated administrators and super-admin users, you must make the azuread-provisioning user a super-admin.

To allow Azure AD to manage non-admin users only, it's sufficient to make the azuread-provisioning user a delegated administrator. As a delegated administrator, Azure AD can't manage other delegated administrators or super-admin users.

Super-admin
Delegated administrator
Note: The super-admin role grants the user full access to Cloud Identity, Google Workspace, and Google Cloud resources.
To make the azuread-provisioning user a super-admin, do the following:

Locate the newly created user in the list and click the user's name to open their account page.
Under Admin roles and privileges, click Assign roles.
Enable the super-admin role.
Click Save.
Warning: To protect the user against credential theft and malicious use, we recommend that you enable 2-step verification for the user. For more details on how to protect super-admin users, see Security best practices for administrator accounts.
Register domains
In Cloud Identity and Google Workspace, users and groups are identified by email address. The domains used by these email addresses must be registered and verified first.

Prepare a list of DNS domains that you need to register:

If you plan to map users by UPN, include all domains used by UPNs. If in doubt, include all custom domains of your Azure AD tenant.
If you plan to map users by email address, include all domains used in email addresses. The list of domains might be different from the list of custom domains of your Azure AD tenant.
If you plan to provision groups, amend the list of DNS domains:

If you plan to map groups by email address, include all domains used in group email addresses. If in doubt, include all custom domains of your Azure AD tenant.
If you plan to map groups by name, include a dedicated subdomain like groups.PRIMARY_DOMAIN, where PRIMARY_DOMAIN is the primary domain name of your Cloud Identity or Google Workspace account.
Now that you've identified the list of DNS domains, you can register any missing domains. For each domain on the list not yet registered, perform the following steps:

In the Admin Console, go to Account > Domains > Manage domains.
Click Add/remove domains.
Click Add a domain.
Enter the domain name and select Secondary domain.
Click Add domain and start verification and follow the instructions to verify ownership of the domain.
Configuring Azure AD provisioning
Create an enterprise application
You are now ready to connect Azure AD to your Cloud Identity or Google Workspace account by setting up the Google Cloud/G Suite Connector by Microsoft gallery app from the Microsoft Azure marketplace.

Note: This app is a Microsoft product and is not maintained or supported by Google.
The gallery app can be configured to handle both user provisioning and single sign-on. In this document, you use two instances of the gallery app—one for user provisioning and one for single sign-on.

First, create an instance of the gallery app to handle user provisioning:

Open the Azure portal and sign in as a user with global administrator privileges.
Select Azure Active Directory > Enterprise applications.
Click New application.
Search for Google Cloud, and then click the Google Cloud/G Suite Connector by Microsoft item in the result list.
Set the name of the application to Google Cloud (Provisioning).
Click Create.
Adding the application may take a few seconds, you should then be redirected to a page titled Google Cloud (Provisioning) - Overview.
In the menu on the left, click Manage > Properties:
Set Enabled for users to sign-in to No.
Set User assignment required to No.
Set Visible to users to No.
Click Save.
In the menu on the left, click Manage > Provisioning:
Click Get started.
Change Provisioning Mode to Automatic.
Click Admin Credentials > Authorize.
Sign in using the azuread-provisioning@DOMAIN user you created earlier, where DOMAIN is the primary domain of your Cloud Identity or Google Workspace account.
Because this is the first time you've signed on using this user, you are asked to accept the Google Terms of Service and privacy policy.
If you agree to the terms, click Accept.
Confirm access to the Cloud Identity API by clicking Allow.
Click Test Connection to verify that Azure AD can successfully authenticate with Cloud Identity or Google Workspace.
Click Save.
Configure user provisioning
The right way to configure user provisioning depends on whether you intend to map users by email address or by UPN.

UPN
UPN: domain substitution
Email address
Under Mappings, click Provision Azure Active Directory Users.
Under Attribute Mapping, select the row userPrincipalName and set Source Attribute to mail.
Select the row surname and set Default value if null to _.
Select the row givenName and set Default value if null to _.
Click OK.
Click Save.
Confirm that saving changes will result in users and groups being resynchronized by clicking Yes.
Click X to close the Attribute Mapping dialog.
You must configure mappings for primaryEmail, name.familyName, name.givenName, and suspended. All other attribute mappings are optional.

When you configure additional attribute mappings, note the following:

The Google Cloud/G Suite Connector by Microsoft gallery currently doesn't let you assign email aliases.
The Google Cloud/G Suite Connector by Microsoft gallery currently doesn't let you assign licenses to users. As a workaround, consider setting up automatic licensing for organizational units.
To assign a user to an organization unit, add a mapping for OrgUnitPath. The path must begin with a / character and must refer to an organizational unit that already exists, for example /employees/engineering.
Configure group provisioning
The right way to configure group provisioning depends on whether your groups are mail-enabled. If groups aren't mail-enabled, or if groups use an email address ending with "onmicrosoft.com", you can derive an email address from the group's name.

No group mapping
Name
Email address
If you map groups by email address, keep the default settings.
Configure user assignment
If you know that only a certain subset of users need access to Google Cloud, you can optionally restrict the set of users to be provisioned by assigning the enterprise app to specific users or groups of users.

If you want all users to be provisioned, you can skip the following steps.

In the menu on the left, click Manage > Users and groups.
Click Add user.
Select Users.
Select the users or groups you want to provision. If you select a group, all members of this group are automatically provisioned.
Click Select.
Click Assign.
Enable automatic provisioning
The next step is to configure Azure AD to automatically provision users to Cloud Identity or Google Workspace:

In the menu on the left, click Manage > Provisioning.
Select Edit provisioning.
Set Provisioning Status to On.
Under Settings, set Scope to one of the following:

Sync only assigned users and groups if you have configured user assignment.
Sync all users and groups otherwise.
If this box to set the scope isn't displayed, click Save and refresh the page.

Click Save.

Azure AD starts an initial synchronization. Depending on the number of users and groups in the directory, this process can take several minutes or hours. You can refresh the browser page to see the status of the synchronization at the bottom of the page or select Audit Logs in the menu to see more details.

After the initial synchronization has completed, Azure AD will periodically propagate updates from Azure AD to your Cloud Identity or Google Workspace account. For further details on how Azure AD handles user and group modifications, see Mapping the user lifecycle and Mapping the group lifecycle.

Troubleshooting
If the synchronization doesn't start within five minutes, you can force it to start by doing the following:

Set Provisioning Status to Off.
Click Save.
Set Provisioning Status to On.
Click Save.
Check Restart provisioning.
Click Save.
Confirm restarting the synchronization by clicking Yes.
If synchronization still doesn't start, click Test Connection to verify that your credentials have been saved successfully.

Configuring Azure AD for single sign-on
Although all relevant Azure AD users are now automatically being provisioned to Cloud Identity or Google Workspace, you cannot use these users to sign in yet. To allow users to sign in, you still need to configure single sign-on.

Create an enterprise application
Create a second enterprise application to handle single sign-on:

In the Azure portal, go to Azure Active Directory > Enterprise applications.
Click New application.
Search for Google Cloud, and then click Google Cloud/G Suite Connector by Microsoft in the result list.
Set the name of the application to Google Cloud.
Click Add.

Adding the application may take a few seconds. You are then redirected to a page titled Google Cloud - Overview.

In the menu on the left, click Manage > Properties.

Set Enabled for users to sign-in to Yes.

Set User assignment required to Yes unless you want to allow all users to use single sign-on.

Click Save.

Configure user assignment
If you already know that only a certain subset of users need access to Google Cloud, you can optionally restrict the set of users to be allowed to sign in by assigning the enterprise app to specific users or groups of users.

If you set User assignment required to No before, then you can skip the following steps.

In the menu on the left, click Manage > Users and groups.
Click Add user.
Select Users and groups/None Selected.
Select the users or groups you want to allow single sign-on for.
Click Select.
Click Assign.
Configure SAML settings
To enable Cloud Identity to use Azure AD for authentication, you must adjust some settings:

In the menu on the left, click Manage > Single sign-on.
On the ballot screen, click the SAML card.
On the Basic SAML Configuration card, click edit Edit.
In the Basic SAML Configuration dialog, enter the following settings:
Identifier (Entity ID): google.com
Reply URL: https://www.google.com/
Sign on URL: https://www.google.com/a/PRIMARY_DOMAIN/ServiceLogin?continue=https://console.cloud.google.com/, replacing PRIMARY_DOMAIN with the primary domain name used by your Cloud Identity or Google Workspace account.
Click Save, and then dismiss the dialog by clicking X.
On the SAML Signing Certificate card, find the entry labeled Certificate (Base 64) and click Download to download the certificate to your local computer.
On the Set up Google Cloud card, look for Login URL. You need this URL shortly.
The remaining steps differ depending on whether you map users by email address or by UPN.

UPN
UPN: domain substitution
Email address
On the User Attributes & Claims card, click edit Edit.
Select the row labeled Unique User Identifier (Name ID).
Change Source attribute to user.mail.
Click Save.
Delete all claims listed under Additional claims. To delete all records, click more_horiz, and then click Delete.

User Attributes & Claims dialog.

Dismiss the dialog by clicking close.

Configuring Cloud Identity or Google Workspace for single sign-on
Now that you've prepared Azure AD for single sign-on, you can enable single sign-on in your Cloud Identity or Google Workspace account:

Open the Admin Console and log in using a super-admin user.
In the menu, click Show more and go to Security > Authentication > SSO with third-party IdP.
Click Add SSO profile.

Note: Don't use the Add SAML profile button.
Set Setup SSO with third party identity provider to enabled.

Enter the following settings:

Sign-in page URL: Enter the Azure AD Login URL. The Login URL is on the Set up Google Cloud card in the Azure Portal under Configuration URLs > Login URL.
Sign-out page URL: https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0
Change password URL: https://account.activedirectory.windowsazure.com/changepassword.aspx
Under Verification certificate, click Upload certificate, and then pick the token signing certificate that you downloaded previously.

Click Save.

Update the SSO settings for the Automation OU to disable single sign-on:

Under Manage SSO profile assignments, click Manage.
Expand Organizational units and select the Automation OU.
Change the SSO profile assignment from Organization's third-party SSO profile to None.
Click Override.
The Azure AD token signing certification is valid only for several months. You must rotate the certificate before it expires. For more information, see Rotate a single sign-on certificate later in this document.

Consider configuring Azure AD to send you notification emails ahead of certificate expiration to avoid certificate expiration from impacting users.

Test single sign-on
Now that you've completed the single sign-on configuration in both Azure AD and Cloud Identity or Google Workspace, you can access Google Cloud in two ways:

Through the list of apps in your Microsoft Office portal.
Directly by opening https://console.cloud.google.com/.
To check that the second option works as intended, run the following test:

Pick an Azure AD user that has been provisioned to Cloud Identity or Google Workspace and that doesn't have super-admin privileges assigned. Users with super-admin privileges always have to sign in using Google credentials and are therefore not suitable for testing single sign-on.
Open a new browser window and go to https://console.cloud.google.com/.
In the Google Sign-In page that appears, enter the email address of the user and click Next. If you use domain substitution, this address must be the email address with the substitution applied.

Google Sign-In dialog.

You are redirected to Azure AD and will see another sign-in prompt. Enter the email address of the user (without domain substitution) and click Next.

Azure AD sign-in dialog.

After entering your password, you are prompted whether to stay signed in or not. For now, choose No.

After successful authentication, Azure AD should redirect you back to Google Sign-In. Because this is the first time you've signed in using this user, you are asked to accept the Google Terms of Service and privacy policy.

If you agree to the terms, click Accept.

You are redirected to the Google Cloud console, which asks you to confirm preferences and accept the Google Cloud Terms of Service.

If you agree to the terms, choose Yes and click Agree and continue.

Click the avatar icon on the top left of the page, and then click Sign out.

You are redirected to an Azure AD page confirming that you have been successfully signed out.

Keep in mind that users with super-admin privileges are exempted from single sign-on, so you can still use the Admin Console to verify or change settings.

Rotate a single sign-on certificate
The Azure AD token signing certificate is valid for only several months, and you must replace the certificate before it expires.

To rotate a signing certificate, add an additional certificate to the Azure AD application:

In the Azure portal, go to Azure Active Directory > Enterprise applications and open the application that you created for single sign-on.
In the menu on the left, click Manage > Single sign-on.
On the SAML Signing Certificate card, click edit Edit.

You see a list of one or more certificates. One certificate is marked as Active.

Click New certificate.

Keep the default signing settings and click Save.

The certificate is added to the list of certificates and is marked as Inactive.

Select the new certificate and click more_horiz > Base64 certificate download.

Keep the browser window open and don't close the dialog.

To use the new certificate, do the following:

Open a new browser tab or window.
Open the Admin Console and log in using a super-admin user.
In the menu, click Show more and go to Security > Authentication > SSO with third-party IdP.
Click SSO profile for your organization.
Click Replace certificate and select the new certificate that you downloaded previously.

Caution: After you save changes, your Cloud Identity or Google Workspace account no longer accepts the old Azure AD signing certificate.
Click Save.

Return to the Azure AD portal and the SAML Signing Certificate dialog.

Select the new certificate and click more_horiz > Make certificate active.

Click Yes to activate the certificate.

Azure AD now uses the new signing certificate.

Test that SSO still works as expected. For more information, see Test single sign-on.

Clean up
To avoid incurring charges to your Google Cloud account for the resources used in this tutorial, either delete the project that contains the resources, or keep the project and delete the individual resources.

To disable single sign-on in your Cloud Identity or Google Workspace account, follow these steps:

Open the Admin Console and log in using the super-admin user created when signing up for Cloud Identity or Google Workspace.
In the menu, go to Security > Settings.
Click Set up single sign-on (SSO) with a third party IdP.
Ensure that Set up SSO with third party identity provider is disabled.
You can remove single sign-on and provisioning settings in Azure AD as follows:

In the Azure portal, go to Azure AD > Enterprise applications.
From the list of applications, choose Google Cloud.
In the menu on the left, click Manage > Single sign-on.
Click Delete.
Confirm the deletion by clicking Yes.