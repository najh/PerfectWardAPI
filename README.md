### API TODO

- [x] API Client
- [x] Auth with credentials file
- [x] JSON - System.Runtime.Serialization.Json
- [x] Hit reports list endpoint - /api/reports
- [x] Hit reports since endpoint - /api/reports?since=...
- [x] Deserialise Report List
- [x] Deserialise Report
- [x] Deserialise Area
- [x] Deserialise Division
- [x] Deserialise InspectionType
- [x] Deserialise Inspector
- [x] Rate limiting
- [x] Deserialise Role
- [x] Deserialise Survey
- [x] Hit detailed report endpoint - /api/report/:id
- [x] Deserialise FinalReflections
- [x] Deserialise Answers
- [x] Activity logging

### Other TODO
- [ ] Setup tool
  - [x] Abstracted DB driver to test connection
  - [x] Abstracted PW wrapper to test connection
  - [ ] Bundle UI + connection tests to provide "easy setup"
  - [ ] Automate scheduled task creation
  - [x] Save down last ID to skip already queried reports.
- [x] Documentation
  - [ ] Finish docs for DB storage
- [ ] Client requirements
  - [x] Windows
  - [x] .NET 4.6
  - [ ] Perfect Ward Account
  - [ ] API key
  - [ ] Firewall access
  - [ ] DB access
- [x] DB backends
  - [x] Separate MSSQL into its own module.
