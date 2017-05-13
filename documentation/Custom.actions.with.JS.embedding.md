**Custom actions with JS embedding**

Custom actions are types of apps experiences where you can develop and deploy them for ribbon control or for menu item to provide new functionality for lists or libraries in a host web to navigate to App web or App web to Host Web. In PnP we have Site Custom actions and Web Custom actions.

Custom actions gets applied while applying provisioning template to site collection. In this step system call ApplyRemoteTemplate function in SitetoTemplateConversion.cs file in OfficeDevPnP.PartnerPack.SiteProvisioning\PnP-Sites-Core-master\Core \OfficeDevPnP.Core \Framework\Provisioning\ObjectHandlers

In this method ObjectHandlerBase class instance Objecthandlers initialization takes place.  Looping through handlers ObjectCustomActions.cs file in Object Handlers has to be executed for both Site Custom actions and web Custom actions.

ProvisionObjects function gets called and it checks for subsite or not. For subsite, Site Custom actions are not enabled. It internally calls ProvisionCustomActionImplementation function.

	if (!web.IsSubSite())
                {
                    var siteCustomActions = template.CustomActions.SiteCustomActions;
                    ProvisionCustomActionImplementation(site, siteCustomActions, parser, scope);
                }

                var webCustomActions = template.CustomActions.WebCustomActions;
                ProvisionCustomActionImplementation(web, webCustomActions, parser, scope);

In ProvisionCustomActionImplementation function properties such as Name, Description, Url, CommandUIExtension, Script Block, Script src, Imageurl, sequence, group, registrationid, registrationtype are set. 
				
				if (!caExists)
                {
                    var customActionEntity = new CustomActionEntity()
                    {
                        CommandUIExtension = customAction.CommandUIExtension != null ? parser.ParseString(customAction.CommandUIExtension.ToString()) : string.Empty,
                        Description = customAction.Description,
                        Group = customAction.Group,
                        ImageUrl = parser.ParseString(customAction.ImageUrl),
                        Location = customAction.Location,
                        Name = customAction.Name,
                        RegistrationId = customAction.RegistrationId,
                        RegistrationType = customAction.RegistrationType,
                        Remove = customAction.Remove,
                        Rights = customAction.Rights,
                        ScriptBlock = parser.ParseString(customAction.ScriptBlock),
                        ScriptSrc = parser.ParseString(customAction.ScriptSrc, "~site", "~sitecollection"),
                        Sequence = customAction.Sequence,
                        Title = customAction.Title,
                        Url = parser.ParseString(customAction.Url)
                    };


                    if (site != null)
                    {
                        scope.LogDebug(CoreResources.Provisioning_ObjectHandlers_CustomActions_Adding_custom_action___0___to_scope_Site, customActionEntity.Name);
                        site.AddCustomAction(customActionEntity);

                        if (customAction.Title.ContainsResourceToken() || customAction.Description.ContainsResourceToken())
                        {
                            var uca = site.GetCustomActions().Where(uc => uc.Name == customAction.Name).FirstOrDefault();
                            SetCustomActionResourceValues(parser, customAction, uca);
                        }

                    }
                    else
                    {
                        scope.LogDebug(CoreResources.Provisioning_ObjectHandlers_CustomActions_Adding_custom_action___0___to_scope_Web, customActionEntity.Name);
                        web.AddCustomAction(customActionEntity);

                        if (customAction.Title.ContainsResourceToken() || customAction.Description.ContainsResourceToken())
                        {
                            var uca = web.GetCustomActions().Where(uc => uc.Name == customAction.Name).FirstOrDefault();
                            SetCustomActionResourceValues(parser, customAction, uca);
                        }

                    }
                }

Based on custom action type whether Site or Web AddCustomAction gets called in NavigationExtensions.cs file in OfficeDevPnP.PartnerPack.SiteProvisioning\PnP-Sites-Core-master\Core\OfficeDevPnP.Core\AppModelExtensions

Here check whether Clientobject is site or not. If it is site then site Custom action gets created, otherwise Web Custom action gets created.

Below code snippet creates custom actions

		if (clientObject is Web)
            {
                var web = (Web)clientObject;

                existingActions = web.UserCustomActions;
                web.Context.Load(existingActions);
                web.Context.ExecuteQueryRetry();

                targetAction = web.UserCustomActions.FirstOrDefault(uca => uca.Name == customAction.Name);
            }
            else
            {
                var site = (Site)clientObject;

                existingActions = site.UserCustomActions;
                site.Context.Load(existingActions);
                site.Context.ExecuteQueryRetry();

                targetAction = site.UserCustomActions.FirstOrDefault(uca => uca.Name == customAction.Name);
            }

