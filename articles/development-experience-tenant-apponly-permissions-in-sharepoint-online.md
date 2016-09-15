Developer Experience using Tenant Permissions in SharePoint Online
================================================
The developer experience has changed for SharePoint Provider-hosted Add-ins that require Tenant permission. This article walks you through the new experience for developing and debugging these solutions. 

_**Applies to:** Add-ins for SharePoint Online_


## Understanding the Problem
In Visual Studio, you navigate to Debug, start debugging and receive a message that "Your tenant administrator has to approve this app" as depicted below.
![](http://i.imgur.com/oFH9oqb.png). 


## Walkthrough
### Step 1: Create a new service principal
Navigate to a site collection in your tenant and generate a new client Id and Secret. (E.g. https://contoso.sharepoint.com/_layouts/15/appregnew.aspx). In this page click Generate for both the Client Id, Client Secret Fields and supply the remaining fields. While you are developing the add-in ensure you use localhost.com including the port as the App Domain. You should have something similar as below.

![](http://i.imgur.com/5CfHgFD.png)

### Step 2: Grant Tenant Permissions
In order to perform this step, you must be a SharePoint Online Administrator. 

Navigate to your SharePoint Admin Center (E.g. https://contoso-admin.sharepoint.com/_layouts/15/appinv.aspx) and grant the tenant permissions
![](http://i.imgur.com/EGuJG3a.png)

![](http://i.imgur.com/dst9ZdP.png)


### Step 3: Update your manifest and web.config
In your Visual Studio solution, update your manifest and web.config with the client id that you created in step 1.
![](http://i.imgur.com/fKkLIde.png)


### Step 4: Package the app and add the .app file to the app catalog
Right click on your SharePoint Add-in Project and click publish. You may need to create publishing profile.

Supply your Client ID and Client Secret that you created in Step 1.

![](http://i.imgur.com/XpM9rwb.png)

Supply your URL for where you are hosting the add-in. Since we want to debug the add-in ensure that you supply https://localhost.com including the port as depicted below.
![](http://i.imgur.com/nQmSbPC.png)

Now deploy your add-in to your App Catalog site.

### Step 5: Install your add-in in your developer site collection

Navigate to your developer site and add the app. Click on App Details.
![](http://i.imgur.com/Aihr4r7.png)

If you clicked on the app tile you will have to click on "Find out why" and request your app
![](http://i.imgur.com/DwWUkG0.png)

Once the request has been submitted the status will be in a pending state until the SharePoint Administrator or the App Catalog Administrator approves the request. To approve the request navigate to the app catalog, App Requests and approve the request.

![](http://i.imgur.com/yZ8vNEc.png)

Once the request has been approved the add-in may not be installed.

![](http://i.imgur.com/PMitOEY.png)

### Step 6: Debug your Add-in
In Visual Studio right click your web project a select Debug Start new instance. Once started navigate to your site and launch the app-in.

![](http://i.imgur.com/Y5vAlDr.png)

## Additional resources
<a name="bk_addresources"> </a>
- [Add-in app only tenant administrative permissions in SharePoint Online](https://msdn.microsoft.com/en-us/pnp_articles/how-to-provide-add-in-app-only-tenant-administrative-permissions-in-sharepoint-online)
- [Add-in permissions in SharePoint 2013](https://msdn.microsoft.com/en-us/library/office/fp142383.aspx)
- [Explore the app manifest structure and the package of a SharePoint Add-in](https://msdn.microsoft.com/en-us/library/office/fp179918.aspx)

