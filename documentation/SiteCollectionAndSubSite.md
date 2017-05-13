**Create Site Collection**

Once Site Collection creation job is created, the status of job changes to Pending. Schedule job picks the list of pending jobs and processes each job. It internally calls RunJob function in ProvisioningJobHandler.cs file in OfficeDevPnP.PartnerPack.SiteProvisioning\OfficeDevPnP.PartnerPack.Infrastructure\Jobs\Handlers. 

Here the job status changes to Running and run the RunJobInternal function in SiteCollectionProvisioningJobHandler.cs.

During this step, CreateSiteCollection function gets called which has SiteCollectionProvisioningJob as parameter. From there the relative url and configuration of site collection stores into an object newsite and calls CreateSiteCollection function in OfficeDevPnP.PartnerPack.SiteProvisioning \PnP-Sites-Core-master\Core\OfficeDevPnP.Core\AppModelExtensions. 

This function checks whether the site is already exists or not then if not exists Site gets created with the properties configured.

Below Code snippets creates site collection

	if (tenant.CheckIfSiteExists(properties.Url, SITE_STATUS_RECYCLED))
                {
                    tenant.DeleteSiteCollectionFromRecycleBin(properties.Url);
                }

	SiteCreationProperties newsite = new SiteCreationProperties();
            newsite.Url = properties.Url;
            newsite.Owner = properties.SiteOwnerLogin;
            newsite.Template = properties.Template;
            newsite.Title = properties.Title;
            newsite.StorageMaximumLevel = properties.StorageMaximumLevel;
            newsite.StorageWarningLevel = properties.StorageWarningLevel;
            newsite.TimeZoneId = properties.TimeZoneId;
            newsite.UserCodeMaximumLevel = properties.UserCodeMaximumLevel;
            newsite.UserCodeWarningLevel = properties.UserCodeWarningLevel;
            newsite.Lcid = properties.Lcid;

            SpoOperation op = tenant.CreateSite(newsite);
            tenant.Context.Load(tenant);
            tenant.Context.Load(op, i => i.IsComplete, i => i.PollingInterval);
            tenant.Context.ExecuteQueryRetry();

**Create Sub Site**

Once Sub Site creation job is created, the status of job changes to Pending. Schedule job picks the list of pending jobs and processes each job. It internally calls RunJob function in ProvisioningJobHandler.cs file in OfficeDevPnP.PartnerPack.SiteProvisioning\ OfficeDevPnP.PartnerPack.Infrastructure\Jobs\Handlers. 

Here the job status changes to Running and run the RunJobInternal function in SubSiteProvisioningJobHandler.cs.

It internally calls CreateSubSite function which has SubSiteProvisioningJob as parameter. It determines the reference url’s and relative paths. Then creates new subsite as child web to the parent web.

Below code snippet creates subsite

				Web parentWeb = context.Web;

                WebCreationInformation newWeb = new WebCreationInformation();
                newWeb.Description = job.Description;
                newWeb.Language = job.Language;
                newWeb.Title = job.SiteTitle;
                newWeb.Url = subSiteUrl;
                newWeb.UseSamePermissionsAsParentSite = job.InheritPermissions;
                newWeb.WebTemplate = PnPPartnerPackSettings.DefaultSiteTemplate;

                Web web = parentWeb.Webs.Add(newWeb);
                context.ExecuteQueryRetry();


