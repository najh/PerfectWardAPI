# Developer Quickstart

## Environment

* Download the latest release library from the [Releases](https://github.com/todo/todo/releases) page.
* Place the library file in an easily accessible location, for example within a `lib` folder in your project.
* Add a reference to this library in your project:
  * From within the `Solution Explorer` pane in Visual Studio, expand the tree item for the current project, and locate the `References` entry.
  * Right click the `References` entry and select `Add Reference...`
  * The `Reference Manager` dialog will appear. Click the `Browse` button from the lower right of the dialog. This will show a file browser dialog. and navigate to the previously downloaded library.
  * Click the `Add` button from the lower right of the file browser dialog, then `OK` from the Reference Manager dialog.

## Generate API Token

* Using the Perfect Ward site and a developer account, obtain an API token from the [Developer Page](https://app.perfectward.com/portal/developer)
 * Keep this token safe, as it cannot be recovered.
 * If the token is lost, it's possible to generate a new one using the `Refresh Token` button. This will invalidate any previously issued tokens for security purposes.

## Authenticate

* Indicate usage of the wrapper code by inserting the following line at the top of the program.

```csharp
using PerfectWardAPI.Api;
```

* Create a new PerfectWardClient object and provide credentials to it:

```csharp
//You can provide credentials as a pair of strings
var creds = new StringCredentials("username@site.com", "yourApiToken");

//Or instead from an INI file
var creds = new IniCredentials("credentials.ini");

//Then supply these credentials to the API wrapper
var pwc = new PerfectWardClient(creds);
```

The INI file, if used, should be formatted as follows:

```ini
USERNAME=username@site.com
APITOKEN=yourApiToken
```

## Fetch Reports

* To fetch detailed report data using the PerfectWardClient object, it's necessary to first determine which reports are available to read.
* PerfectWardClient operates asynchronously and as such its methods should be awaited:

```csharp
var creds = new IniCredentials("credentials.ini");
var pwc = new PerfectWardClient(creds);

Task.Factory.StartNew(async () =>
{
    if(await pwc.TestApi()) //Test for successful authentication.
    {
        var reportsResponse = (await pwc.ListReports()).Reports;
        var details = await Task.WhenAll(reports.Select(async x => await pwc.ReportDetails(x.Id)));
        //Operate on details array here.
    }
});
```

* The API provides a simplified list of reports when queried with `ListReports`. The details of each report are then pulled in using `ReportDetails`, providing the ID of the desired report.

## Optimise

* It's not always necessary to gather the entire set of reports. When iterating through the list of reports, it's possible to fetch the unix timestamp of the most recent report:

```csharp
var maxTimestamp = details.Max(x => x.EndedAtDate).ToTimeStamp();
```

* This value can then be used to only query new reports from the API:

```csharp
var reportsResponse = (await pwc.ListReportsSince(maxTimestamp)).Reports;
```

### Pagination

* Results are served in pages, where the page size can be specified. By default, queries will provide a page number of `1`, which can be incremented to access subsequent pages.
* The pagination information is returned in the [Meta](meta.md) part of a response:

```csharp
var maxTimestamp = driver.GetLatestEndDate().ToTimeStamp();
var reportsResponse = await pwc.ListReportsSince(maxTimestamp);
var reports = reportsResponse.Reports;
if(reportsResponse.Meta != null)
{
    int currentPage = reportsResponse.Meta.CurrentPage;
    int totalPages = reportsResponse.Meta.TotalPages;
    while (currentPage < totalPages)
    {
        reportsResponse = await pwc.ListReportsSince(maxTimestamp, ++currentPage);
        reports.AddRange(reportsResponse.Reports);
    }
}
```

* Duplicates may be found if live data changes occur when iterating pages and should be filtered out:

```csharp
reports = reports.Distinct().ToList();
```

## Store

* The wrapper supports storing reports in a database through the `IDbDriver` interface.

```csharp
var connStr = @"Data Source=...;Initial Catalog=...;etc";
var driver = DbDriver.Create(connStr);
```

* Storing the connection string in source code is not advised. The use of a file or environment variable is preferred.
* DbDriver will locate the first DLL in the current directory that matches the file wildcard `*Driver.dll`
* Upon finding such a file, the assembly is loaded and queried for the first class that implements `IDbDriver`
* This class is instantiated and returned, ready to query and store data:

```csharp
var driver = DbDriver.Create(connStr);

if(driver == null)
{
    //No DB driver found
    return;
}

if (driver.TestConnection())
{
    var creds = new IniCredentials("credentials.ini");
    var pwc = new PerfectWardClient(creds);

    Task.Run(async () =>
    {
        if (await pwc.TestApi())
        {
            var maxTimestamp = driver.GetLatestEndDate().ToTimeStamp();
            var reportsResponse = await pwc.ListReportsSince(maxTimestamp);
            var reports = reportsResponse.Reports;
            if(reportsResponse.Meta != null)    //Pagination
            {
                int currentPage = reportsResponse.Meta.CurrentPage;
                int totalPages = reportsResponse.Meta.TotalPages;
                while (currentPage < totalPages)
                {
                    reportsResponse = await pwc.ListReportsSince(maxTimestamp, ++currentPage);
                    reports.AddRange(reportsResponse.Reports);
                }
            }
            reports = reports.Distinct().ToList();
            var details = await Task.WhenAll(reports.Select(async x => await pwc.ReportDetails(x.Id)));
            driver.UploadReports(ref details);
        }
    }).Wait();
}
```

## Summary

* The ideal workflow for accessing and storing report data can be described as follows:
  * Create an instance of a database driver.
  * If the database reports as accessible, create an API client and authenticate.
  * If the API reports as accessible, determine the latest report date.
  * Use the report date to perform a filtered query for report summaries.
  * For each page of responses, fetch all report summaries.
  * For each report summary, fetch a detailed report.
  * Pass these detailed reports to the database for storage.
