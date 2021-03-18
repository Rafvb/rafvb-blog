+++
title = "Adding this site to the Google search engine"
tags = [
    "azure",
    "blog",
    "google"
]
date = 2021-03-22
+++

This new blog deserves to be found by the most popular search engine on the planet.

To add it I had to [verify my ownership](https://search.google.com/search-console/ownership) of the 'property'.
I added a new property in the Google Search Console, which prompted me to add a txt-record to my DNS config.

![Adding a property to Google Search Console](/adding-site-to-google/google-search-console-add-property.png)

I copied the value from the popup, went to my App Service Domain's DNS zone in Azure and added a record set:

![Add new Records set in the Azure Portal](/adding-custom-domain-to-static-web-app/azure_add_record_set.png)

Enter these values (leave the defaults for the others):

* Name: @
* Type: TXT
* Value: The value you copied from the Google Search Console

Back in the Google Search Console, the verification should work now.

According to Google you should leave the record in so they verify it again later.

After some time the first results should be visible in the Search Console 👍

You can also provide a link to a sitemap to Google, so it can easily index all your pages. Hugo will generate one automatically at [/sitemap.xml](https://www.rafvanbaelen.com/sitemap.xml). You can add it in the Search Console under Index > Sitemaps by entering the full url of your sitemap.xml.