# Report

## Summary
* Represents both a report summary and detailed report.
* Contains data relating to an inspection.

## Fields

| Name  | Type | Description |
|---|---|---|
| Id | int | ID of the report |
| Score | float | Result of the inspection, ranging from 0-100 |
| Comment | string | Comments on the inspection |
| StartedAtDate | DateTime | When the inspection began |
| EndedAtDate | DateTime | When the inspection ended |
| InspectionType | [InspectionType](inspection_type.md) | The type of inspection performed |
| Area | [Area](area.md) | Details of where the inspection was performed |
| Inspector | [Inspector](inspector.md) | By whom the inspection was performed |
| Survey | [Survey](survey.md) | Details relating to the overall set of inspections |
| FinalReflections | [FinalReflection[]](final_reflection.md) | Comments on the inspection |
| Answers | [Answer[]](answer.md) | Responses given to the inspection questions |