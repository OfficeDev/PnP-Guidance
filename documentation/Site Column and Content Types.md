**Create Site Columns**

Site columns gets created while applying provisioning template to site collection. In this step system calls ApplyRemoteTemplate function in SitetoTemplateConversion.cs file in OfficeDevPnP.PartnerPack.SiteProvisioning\PnP-Sites-Core-master\Core\OfficeDevPnP.Core\Framework\Provisioning\ObjectHandlers

In this method ObjectHandlerBase class instance Objecthandlers initialization takes place. Looping through handlers ObjectFiled.cs file in Object Handlers has to be executed for Creation of Site Column.

ProvisionObjects function in ObjectFiled.cs calls the CreateFiled function internally after getting template site fields.

***var fields = template.SiteFields;*** //Gets the Site Column field from template

Looping through site fields, extract the templatefieldelement schema and checks the id attribute with existing fields id. If the current id is not in the existing field id's list then call CreateField function with schemaxml, templatefield element as parameters and Site column gets created.

Below Code snippet is the one which creates site column.

	var fieldXml = parser.ParseString(templateFieldElement.ToString(), "~sitecollection", "~site");
	var field = web.Fields.AddFieldAsXml(fieldXml, false, AddFieldOptions.AddFieldInternalNameHint);
	web.Context.Load(field, f => f.TypeAsString, f => f.DefaultValue);
	web.Context.ExecuteQueryRetry();

------
**Creating Content Type**

Content Type gets created while applying provisioning template to site collection. In this step system calls ApplyRemoteTemplate function in SitetoTemplateConversion.cs file in OfficeDevPnP.PartnerPack.SiteProvisioning\PnP-Sites-Core-master\Core\OfficeDevPnP.Core\Framework\Provisioning\ObjectHandlers

In this method ObjectHandlerBase class instance Objecthandlers initialization takes place. To Create Content Type, ObjectContentType.cs file in Object Handlers has to be executed.

ProvisionObjects function in ObjectContentType.cs calls CreateContentType function internally after getting the collection of content types to create ordered by Id which handle references to parent content types that can be in same template. 

CreateContentType function in ObjectContentType.cs stores information of content type like name, description, id, group and  calls CreateContentType function in OfficeDevPnP.PartnerPack.SiteProvisioning\PnP-Sites-Core-master\Core\OfficeDevPnP.Core \AppModelExtensions\ Fieldandcontenttypeextensions.cs

***var createdCT = web.CreateContentType(name, description, id, group);*** //
calling CreateContentType function in Fieldandcontenttypeextensions.cs

Below code snippet creates content type
     
			var contentTypes = web.ContentTypes;
            var newCt = new ContentTypeCreationInformation();
            // Set the properties for the content type
            newCt.Name = name;
            newCt.Id = id;
            newCt.Description = description;
            newCt.Group = group;
            newCt.ParentContentType = parentContentType;
            var myContentType = contentTypes.Add(newCt);
            web.Context.ExecuteQueryRetry();




