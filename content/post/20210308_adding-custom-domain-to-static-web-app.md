+++
title = "Adding a custom domain to a Static Web App"
tags = [
    "azure",
    "blog",
]
date = 2021-03-08
+++

In the [previous post]({{< ref "/post/20210301_blogging-with-hugo-on-azure.md" >}}) I created a new blog on Azure using a Static Web App.
By default you get a funky auto genererated url by azure. To make feel more like home we can add a custom domain.

Since I was going all in on Azure, I also wanted to try their domain service (which is GoDaddy on the backend I believe). 

## Adding a custom domain

First we need to add a new resource called App Service Domain, following [these steps](https://docs.microsoft.com/en-us/azure/app-service/manage-custom-dns-buy-domain?WT.mc_id=Portal-Microsoft_Azure_Marketplace).

To add the custom domain to our Static Web App we need to add a CNAME record to the DNS records of our newly created domain:

![Manage DNS records in the Azure Portal](/adding-custom-domain-to-static-web-app/azure_manage_dns_records.png)

On the DNS zone, add a new Record set:

![Add new Records set in the Azure Portal](/adding-custom-domain-to-static-web-app/azure_add_record_set.png)

Enter these values (leave the defaults for the others):

* Name: www
* Type: CNAME
* Alias: <your-azure-static-app-name>.azurestaticapps.net

The last one is the url of your Static Web App.

Lastly we can couple the Domain to our Static Web App. In the Static Web App click Custom Domains and + Add.
Enter your custom domain (eg. www.rafvanbaelen.com) and click next. 
With CNAME selected you can click validate and the Hostname should be added. If validation fails make sure your CNAME matches what is shown in the Validate domain ownership step.

Also update the baseUrl setting in your config.yml.

That should do the trick, the site should be reachable via the new domain name. Fancy!