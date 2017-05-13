**Connect-SPOService**

To get started, Sharepoint Online Management Shell, Windows Management Framework 3.0 has to be installed. It is a Windows Powershell module that you can use to efficiently manage SharePoint Online users, sites, site collections, and organizations. Command-line operations in Windows PowerShell are composed of cmdlets. 

To connect to Sharepoint Online, we have to use Connect-SPOService cmdlet. It connects a SharePoint Online global administrator to the SharePoint Online Administration Center. 

Perform the below steps to connect to Sharepoint Online:

1.	Open Sharepoint Online Management Shell by Clicking Start -> All Programs -> Sharepoint Online Management Shell.

2.	Right click on that and Run as Administrator.

3.	Import the Sharepoint module by executing the below command:
             Import-Module Microsoft.Online.SharePoint.PowerShell –DisableNameChecking

4.	Run the following command

**Connect-SPOService -Url https://customerdomain-admin.sharepoint.com -credential admin@customerdomain.com**

Where Url is URL of the SharePoint Online Admin Centre
Credential is user that has admin access to the SharePoint Online service
5.	If you connect successfully, you will be returned to the command prompt without error. 


Only a single SharePoint Online service connection is maintained from any single Windows PowerShell session. Running the Connect-SPOService cmdlet twice implicitly disconnects the previous connection.

