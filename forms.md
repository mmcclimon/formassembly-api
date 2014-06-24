# API Reference - Forms

## Index
+ https://app.formassembly.com/api_v1/forms/index.json
+ https://app.formassembly.com/api_v1/forms/index.xml

Returns a list of the forms in the user's account, along with associated metadata.

## Admin Index (Enterprise Plan only)
 + https://app.formassembly.com/admin/api_v1/forms/index.json
 + https://app.formassembly.com/admin/api_v1/forms/index.xml

Returns a list of all forms in the FormAssembly instance. Only accessible if using FormAssembly Enterprise and an access token from an admin-level user.

## Code Examples

 + PHP: https://github.com/veerwest/formassembly-api/blob/master/php/fa_api.php
 + Python (command-line): https://github.com/veerwest/formassembly-api/blob/master/python/fa_api.py
 + Bash (command-line): https://github.com/veerwest/formassembly-api/blob/master/curl/fa_api.sh
 + Salesforce: https://github.com/drewbuschhorn/sfdc-oauth-playground/tree/oauth2_drew

***

## Object Reference

Object | Description | Example
---: | --- | :---
"id":"XXXX" | Unique integer value identifying the form within the FormAssembly instance. Every form has a single unique ID in the form of an integer. Can be used to construct a valid form URL, e.g., http://app.formassembly.com/forms/view/XXXX | 1
"version_id":"XXXX" | Unique integer ID identifying the current version (revision) of the form. | 1
"name":"XXXXXXXX" | HTML-encoded string representing the form's name as displayed in the user's FormAssembly form index list. Not to be confused with the Form Title, found in the form's XML definition. | "Mine &amp; Yours Form"
"category":"XXXX", | String representing one of the system-wide default form organizational categories. Can be an empty string for uncategorized forms. | "Contact Forms"
"subcategory":"XXXX", | String representing one of the user-created organizational categories. Can be an empty string for uncategorized forms. | "IBM Contact Forms"
"is_template":"XXXX", | Integer specifying whether or not the form is shared as a template publicly. Contact Support for more details.<br/><br/>**Values**<br/>`0`: Not a template<br/>`>1`: Is a template | 0
"display_status":"XXXX", |Integer specifying whether or not the form is active or archived.<br/><br/>**Values**<br/>`0`:  Archived<br/>`2`: Active | 2
"moderation_status":"XXXX", | Integer specifying whether or not the form is moderated (under review for suspicious content).<br/><br/>**Values**<br/>`0`: Not checked<br/>`2`: Reviewed and approved<br/>`3`: Reviewed and denied | 0
"use_ssl":"XXXX", | Integer representing if the form must be displayed over HTTPS. If true, form URL must contain `https://`. | |
"created":"XXXXXXXX" | String timestamp representing the date the form was created. | "created":"1983-01-01 23:59:59"
"modified":"XXXXXXXX" | String timestamp representing the last time the form or any of its settings were modified. | "modified":"2012-06-17 17:50:12"
"expired":"XXXX" | String timestamp representing the date the form was marked for deletion, if any. | "expired":null
