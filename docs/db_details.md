# Database Details

## Summary {docsify-ignore}

* The API connector has a simple schema when storing reports.

## Tables {docsify-ignore}

* Two tables are created to store report information:
  * `reports` - Stores report information
  * `answers` - Stores answers given to a report survey

## Reports Table {docsify-ignore}

| Name  | Type | Description |
|---|---|---|
| id | bigint | ID of the report |
| score | float | Percentage score for the inspection. |
| comment | varchar(500) | Comments on the inspection |
| started_at | dateTime | When the inspection began |
| ended_at | dateTime | When the inspection ended |
| inspection_type_id | bigint | ID of the inspection type |
| inspection_type_name | varchar(100) | Name of the inspection type |
| area_id | bigint | ID of the inspection area |
| area_name | varchar(100) | Name of the inspection area |
| area_division_id | bigint | ID of the area division |
| area_division_name | varchar(100) | Name of the area division |
| inspector_id | bigint | ID of the inspector |
| inspector_name | varchar(100) | Name of the inspector |
| inspector_email | varchar(100) | Email of the inspector |
| inspector_first_name | varchar(100) | First name of the inspector |
| inspector_last_name | varchar(100) | Last name of the inspector |
| inspector_job_title | varchar(100) | Job title of the inspector |
| inspector_role_id | bigint | Role ID of the inspector |
| inspector_role_name | varchar(100) | Role name of the inspector |
| survey_id | bigint | ID of the survey |
| survey_name | varchar(100) | Name of the survey |
| final_reflections | varchar(1000) | Final reflections of the report |

## Answers Table {docsify-ignore}


| Name  | Type | Description |
|---|---|---|
| id | bigint | ID of the answer |
| report_id | bigint | ID of the report |
| score | float | Score given for the answer |
| it_scores | bit | Whether this answer's score affects the report score |
| is_multiple | bit | Whether there are multiple answers provided |
| answer_text | text | The contents of the answer |
| answer_choice_id | int | The chosen answer, or `null` |
| question_id | bigint | ID of the question |
| question_text | text | The contents of the question |
| category_name | text | The name of the question category |
| note | text | Notes taken in regards to the response given |
| parent_id | bigint | The parent answer id or `null` |