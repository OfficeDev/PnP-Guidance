Title: Upload large files sample app for SharePointDescription: As part of your Enterprise Content Management (ECM) strategy, you can use different methods to upload large files to a SharePoint document library.
ms.ContentId: e21d224b-5fac-4d04-af99-5e658c07bd5c


# Upload large files sample app for SharePoint
As part of your Enterprise Content Management (ECM) strategy, you can use different methods to upload large files to a SharePoint document library.

    
 _**Applies to:** Office 365 | SharePoint 2013 | SharePoint Online_

    

    

- The  **SaveBinaryDirect** method on the Microsoft.SharePoint.Client.File object
    
- The ContentStream property on the FileCreationInformation object
    
Use this solution if you want to upload files that are larger than 2 MB to SharePoint.
 **Contribute to this content**

    

## Before you begin
<a name="sectionSection0"> </a>

To get started, download the  [Core.LargeFileUpload](https://github.com/OfficeDev/PnP/tree/dev/Samples/Core.LargeFileUpload) sample app from the [Office 365 Developer patterns and practices](https://github.com/OfficeDev/PnP/tree/dev) project on GitHub.


## Using the Core.LargeFileUpload sample app
<a name="sectionSection1"> </a>

When you start this app, a console application appears, as shown in Figure 1. You must supply a SharePoint Online site collection URL and your logon credentials for Office 365.


**Figure 1. Core.LargeFileUpload console application**

![Screenshot that shows a console application with user name and password logon credentials.](media/09de7f68-b021-4ca8-ae82-0ca26427b856.png)After authentication, the console application displays an error, as shown in Figure 2. The error occurs when the  **UploadDocumentContent** method in FileUploadService.cs tries to use the **FileCreationInformation.Content** property to upload a file that is larger than 2 MB, and an exception is thrown.


    
**Tip**  Avoid using the  **FileCreationInformation.Content** property in your code because it restricts your file upload size to a maximum of 2 MB.


**Figure 2. Exception that is thrown when using the FileCreationInformation.Content property to upload files larger than 2 MB**

![Screenshot of the error message that appears when an attempt is made to upload a file larger than 2 MBs.](media/ccc6e053-aff4-43e3-9d8a-24a345aaf92d.png)
    
**Note**  The code in this article is provided as-is, without warranty of any kind, either express or implied, including any implied warranties of fitness for a particular purpose, merchantability, or non-infringement.




```C#
public void UploadDocumentContent(ClientContext ctx, string libraryName, string filePath)
        {
            Web web = ctx.Web;

            // Ensure that target library exists. Create if it is missing.
            if (!LibraryExists(ctx, web, libraryName))
            {
                CreateLibrary(ctx, web, libraryName);
            }

            FileCreationInformation newFile = new FileCreationInformation();

		 // The next line of code causes an exception to be thrown for files larger than 2 MB.
            newFile.Content = System.IO.File.ReadAllBytes(filePath);
            newFile.Url = System.IO.Path.GetFileName(filePath);

            // Get instances to the given library.
            List docs = web.Lists.GetByTitle(libraryName);
            
 // Add file to the library.
            Microsoft.SharePoint.Client.File uploadFile = docs.RootFolder.Files.Add(newFile);
            ctx.Load(uploadFile);
            ctx.ExecuteQuery();
        } 

```

 **UploadDocumentContent** creates a document library called Docs if it does not already exist. The Docs document library is used later in this code sample.

The Core.LargeFileUpload sample provides two options that you can use to upload large files to a document library:



- The  **SaveBinaryDirect** method
    
- The  **UploadDocumentContentStream** method
    
In the following code,  **SaveBinaryDirect** in FileUploadService.cs uses the **Microsoft.SharePoint.Client.File.SaveBinaryDirect** method with a **FileStream** object to upload files to a document library.




```C#
public void SaveBinaryDirect(ClientContext ctx, string libraryName, string filePath)
        {
            Web web = ctx.Web;
            // Ensure that the target library exists. Create it if it is missing.
            if (!LibraryExists(ctx, web, libraryName))
            {
                CreateLibrary(ctx, web, libraryName);
            }

            using (FileStream fs = new FileStream(filePath, FileMode.Open))
            {
                Microsoft.SharePoint.Client.File.SaveBinaryDirect(ctx, string.Format("/{0}/{1}", libraryName, System.IO.Path.GetFileName(filePath)), fs, true);
            }

        } 

```

In the following code,  **UploadDocumentContentStream** in FileUploadService.cs uses the **FileCreationInformation.ContentStream** property with the **Microsoft.SharePoint.Client.File** object to upload a file to a document library. Assign a **FileStream** object to the **FileCreationInformation.ContentStream** property.




```C#
public void UploadDocumentContentStream(ClientContext ctx, string libraryName, string filePath)
        {

            Web web = ctx.Web;
            // Ensure that the target library exists. Create it if it is missing.
            if (!LibraryExists(ctx, web, libraryName))
            {
                CreateLibrary(ctx, web, libraryName);
            }

            using (FileStream fs = new FileStream(filePath, FileMode.Open))
            {
                FileCreationInformation flciNewFile = new FileCreationInformation();

                // This is the key difference for the first case - using ContentStream property
                flciNewFile.ContentStream = fs;
                flciNewFile.Url = System.IO.Path.GetFileName(filePath);
                flciNewFile.Overwrite = true;

                List docs = web.Lists.GetByTitle(libraryName);
                Microsoft.SharePoint.Client.File uploadFile = docs.RootFolder.Files.Add(flciNewFile);

                ctx.Load(uploadFile);
                ctx.ExecuteQuery();
            }
        }

```

After the console application runs, you can go to the Docs document library by choosing  **Recent** > **Docs**. The document library contains two large files. 


**Figure 3. Document document library with two large files**

![Screenshot of a document library that includes two large files.](media/51902366-2d57-4b19-81c2-eb4047b868bc.png)


## Additional resources
<a name="bk_addresources"> </a>


-  [Enterprise Content Management solutions for SharePoint 2013 and SharePoint Online](..\api\Enterprise-Content-Management-solutions-for-SharePoint-2013-and-SharePoint-Online.md)
    
    
    
-  [Core.BulkDocumentUploader sample](https://github.com/OfficeDev/PnP/tree/dev/Samples/Core.BulkDocumentUploader)
    
