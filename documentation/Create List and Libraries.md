**Create List and Libraries**

List and library gets created while applying provisioning template to site collection. In this step system call ApplyRemoteTemplate function in SitetoTemplateConversion.cs file in OfficeDevPnP.PartnerPack.SiteProvisioning\PnP-Sites-Core-master\Core\OfficeDevPnP.Core\ Framework\Provisioning\ObjectHandlers

In this method ObjectHandlerBase class instance Objecthandlers initialization takes place.  Looping through handlers ObjectListInstance.cs file in Object Handlers has to be executed for Creation of List and Library.

ProvisionObjects function gets called and in this retrieves the information of List and Library from template with the below line of code.

	template.Lists.Any()

If template has list or library that has to be created then it goes through if condition and calls CreateList function. Here the information like description, templatetype, title gets extracted and On-Quick launch option is reset on created list object. Then creates the List if **templatetype is 100** and creates Library if **templatetype is 101**.

Below code snippet creates the List and Library

	        var listCreate = new ListCreationInformation();
            listCreate.Description = list.Description;
            listCreate.TemplateType = list.TemplateType;
            listCreate.Title = parser.ParseString(list.Title);
            listCreate.QuickLaunchOption = list.OnQuickLaunch ? QuickLaunchOptions.On :    QuickLaunchOptions.Off;
            listCreate.Url = parser.ParseString(list.Url);
            listCreate.TemplateFeatureId = list.TemplateFeatureID;
            var createdList = web.Lists.Add(listCreate);
            createdList.Update();
            web.Context.Load(createdList, l => l.BaseTemplate);
            web.Context.ExecuteQueryRetry();


If List instance list has document template then sets the documenttemplateurl.

	if (!String.IsNullOrEmpty(list.DocumentTemplate))
            {
                createdList.DocumentTemplateUrl = parser.ParseString(list.DocumentTemplate);
            }

Finally, fieldreference, fields, default field values, views and folders are set to the newly created list and libraries.


