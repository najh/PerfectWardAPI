# Getting started

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
    var details = new List<Report>();
    var reportsResponse = (await pwc.ListReports()).Reports;
    foreach (var report in reportsResponse)
    {
        var detailedReportResponse = await pwc.ReportDetails(report.Id);
        details.Add(detailedReportResponse.Report);
    }

    //Operate on details list here.
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

## Store

TODO:
* Describe how a developer might store report data locally, or in a database.
* Document both the generation of JSON files and the SQL Server helper class.