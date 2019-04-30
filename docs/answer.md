# Answer

## Summary {docsify-ignore}
* Represents an answer and sub-answers given to an inspection.

## Fields {docsify-ignore}

| Name  | Type | Description |
|---|---|---|
| Id | int | ID of the answer |
| Score | float | The score given to the answer |
| ItScores | bool | Whether this answer contributes towards the average score |
| IsMultiple | bool | Whether there are multiple answers provided |
| AnswerText | string | The contents of the answer |
| AnswerChoiceId | int? | The chosen answer, or `null` |
| QuestionId | int | ID of the question |
| QuestionText | string | The contents of the question |
| CategoryName | string | The name of the question category |
| Note | string | Notes taken in regards to the response given |
| SubAnswers | [Answer[]](answer.md) | Sub-answers given when multiple answers are available |
