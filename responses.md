#### Responses [Returned Fields Reference]

##### Export
+ https://app.formassembly.com/api_v1/responses/export/#FORMID#.csv
+ https://app.formassembly.com/api_v1/responses/export/#FORMID#.json
+ https://app.formassembly.com/api_v1/responses/export/#FORMID#.xml
+ https://app.formassembly.com/api_v1/responses/export/#FORMID#.zip

##### Additional Parameters
+ `date_from`: Start date for export range.
+ `date_to`: End date for export range.
+ `filter`: If set to `all`, export will include both completed and incomplete responses.
+ `response_ids`: Set of comma-delimited response IDs to retrieve.

##### Examples
+ https://app.formassembly.com/api_v1/responses/export/1.csv?date_form=01/01/2012&date_to=01/01/2013&filter=all
  * would retrieve all responses (including incompletes) created between January 1, 2012 and January 1, 2013.
+ https://app.formassembly.com/api_v1/responses/export/1.xml?response_ids=10,11,12
  * would retrieve only responses with IDs: 10, 11, 12.
