**Apply Branding Elements**

Branding Elements gets applied while applying provisioning template to site collection. In this step system call ApplyRemoteTemplate function in SitetoTemplateConversion.cs file in OfficeDevPnP.PartnerPack.SiteProvisioning\PnP-Sites-Core-master\Core\OfficeDevPnP.Core\Framework\Provisioning\ObjectHandlers

In this method ObjectHandlerBase class instance Objecthandlers initialization takes place.  Looping through handlers ObjectComposedLook.cs file in Object Handlers has to be executed for applying Branding elements.

ProvisionObjects function gets called and it checks for template.ComposedLook property. If Composedlook properties such as Color file, Font file and Backgroundfile are null then OOB theme gets applied otherwise custom theme gets applied.

Below code snippet applies Branding elements

   			if (String.IsNullOrEmpty(template.ComposedLook.ColorFile) &&
            String.IsNullOrEmpty(template.ComposedLook.FontFile) &&
            String.IsNullOrEmpty(template.ComposedLook.BackgroundFile))
            {
             // Apply OOB theme
            web.SetComposedLookByUrl(template.ComposedLook.Name);
            }
            else
            {
             // Apply custom theme
             string colorFile = null;
             if (!string.IsNullOrEmpty(template.ComposedLook.ColorFile))
             {
              colorFile = parser.ParseString(template.ComposedLook.ColorFile);
             }
             string backgroundFile = null;
             if (!string.IsNullOrEmpty(template.ComposedLook.BackgroundFile))
             {
              backgroundFile = parser.ParseString(template.ComposedLook.BackgroundFile);
             }
             string fontFile = null;
             if (!string.IsNullOrEmpty(template.ComposedLook.FontFile))
             {
             fontFile = parser.ParseString(template.ComposedLook.FontFile);
             }

             string masterUrl = null;
             if (template.WebSettings != null && !string.IsNullOrEmpty(template.WebSettings.MasterPageUrl))
             {
              masterUrl = parser.ParseString(template.WebSettings.MasterPageUrl);
             }
             web.CreateComposedLookByUrl(template.ComposedLook.Name, colorFile, fontFile, backgroundFile, masterUrl);
             web.SetComposedLookByUrl(template.ComposedLook.Name, colorFile, fontFile, backgroundFile, masterUrl);

             var composedLookJson = JsonConvert.SerializeObject(template.ComposedLook);

             web.SetPropertyBagValue("_PnP_ProvisioningTemplateComposedLookInfo", composedLookJson);
             }


