# IDbDriver

## Summary
A simple interface to abstract report storage and querying for the latest `Ended At` date.

## Methods

### TestConnection
* Returns `true` when a successful connection can be made to the database, else `false`.
* It is preferred that the class implementing IDbDriver accepts any connection settings or similar in the constructor.

### GetLatestEndDate
* Returns the `DateTime` associated with the most recently stored `Report`, else `DateTime.MinValue`

### UploadReports
* Accepts a reference to a collection of `DetailedReportResponse`
* Returns nothing, uploads reports from the collection to the database.
