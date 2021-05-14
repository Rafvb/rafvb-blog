+++
title = "YNAB utilities"
tags = [
    "YNAB",
    "azure",
    "blazor",
    "F#"
]
date = 2021-05-14
+++

## YNAB
A colleague of mine pointed me to the fabulous [YNAB](https://www.youneedabudget.com/) budgetting app. I've tried some budgetting apps over the last years, but never stuck with them. Currently I am using YNAB for over a year now and I'm really ❤ it. The way it deals with budgetting (label all your money) took a few moments to click, but when it does it makes it really easy (and fun) to take control over your budget.

Now this is not some kind of infomercial for YNAB (but still, you should give it a try 😉), but a technical blog post on a technical blog, so get technical already!

## Importing
YNAB has a direct importing tool, but that (currently) only works for a limited number of banks, mostly American. My 💵 is stored at BNP Paribas Fortis, so I need to import from there. I had an excel file set up that parsed the BNP format into the YNAB one, but wanted to it simpler (or more technically over engineered 😋).

I thought about setting up an Azure Function to do the parsing, but that would mean sending my banking data into the cloud, so I decided to make something that keeps the data locally on my machine (until I upload it to YNAB of course).

So the setup I went with is:
* An F# library that uses FSharp.Data to parse and create csv's
* A Blazor client side website that handles the file access
* Azure Static Web Apps to host it all

You can find the code on GitHub: [https://github.com/Rafvb/YNAB](https://github.com/Rafvb/YNAB)

## FSharp.Data
The parsing library uses [FSharp.Data](https://fsprojects.github.io/FSharp.Data/) to handle the csv's.

I embed a sample.csv file containing the BNP format and a ynab.csv file containing the YNAB format. I can then create a type using the CsvProvider. This will inspect the csv and give me intellisense on the type based on the info in the csv! Magic 🎇 

```
type Transactions = CsvProvider<"sample.csv", EmbeddedResource="Raf.YNAB.Importer, Raf.YNAB.Importer.sample.csv", Separators=";", Culture="nl-be">
```

I let FSharp.Data figure out the columns in the BNP file, but had to help a bit with the YNAB format.

```
type YNABTransactions = CsvProvider<"ynab.csv", Schema="Date (string), Payee (string), Memo (string), Outflow (string), Inflow (string)",
                                                EmbeddedResource="Raf.YNAB.Importer, Raf.YNAB.Importer.ynab.csv">
```

Because I am creating a library and had some problems when I used my library from my tests or the Blazor client. FSharp.Data needs to inspect the csv file and when it's code is embedded in a library you have to help it find the input files. This can be done by specifying the EmbeddedResource as seen above in the CsvProviders.
Make sure to add the full names, including the namespace (eg _"Raf.YNAB.Importer, Raf.YNAB.Importer.ynab.csv"_)

## Blazor
For the front-end I used a simple blazor app. It's a client side app, so it runs locally in your browser.

It accepts a csv file and when succesfully parsed will download you the YNAB version. It feels funky to be reading streams in C# in the client in the browser, but it all works 😁 To directly download the result file we have to call out to a javascript function (defined in the index.html).

```
await js.InvokeAsync<object>("saveAsFile", fileName, Convert.ToBase64String(bytes));
```

# Azure Static Web apps
Again (same as this blog), I threw the app onto Azure Static Web Apps. It's just really easy and the GitHub Actions integration is nice.
Every time I push to the repo it will do a nice little CI-CD dance building, testing and publishing this simple app.

The app is up at [https://lively-hill-0c0777603.azurestaticapps.net/](https://lively-hill-0c0777603.azurestaticapps.net/).