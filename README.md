# The FormAssembly API

## Introduction

[FormAssembly.com](http://formassembly.com) is a web-based form management solution that allows anyone to easily create online forms and collect data. FormAssembly is available in different flavors, cloud-hosted (SaaS) or on-premises, and all editions provide a secure REST API for interacting with user accounts and exporting data.

Before you can start building an application using the FormAssembly API, check out the [FormAssembly Developer Hub](http://www3.formassembly.com/api/) for information on how to register, obtain a CLIENT_ID, and get access to a sandbox.

***

## Endpoint

The **endpoint**, in API parlance, is the destination URL for your API requests. The endpoint will vary depending on the FormAssembly edition you're interacting with.

| Edition  | Endpoint | Note |
| -------- | -------- | ---- |
| FormAssembly Developer Sandbox | https://developer.formassembly.com/api_v1/ | For development and testing only |
| FormAssembly.com | https://app.formassembly.com/api_v1/ | |
| FormAssembly Enterprise Cloud | https://instance_name.tfaforms.net/api_v1/   | Adjust `instance_name` as needed |
| FormAssembly Enterprise On-Site | https://your_server/path/to/formassembly/api_v1/  | Adjust `your_server` and `path` as needed |

From this point on, we'll use the FormAssembly.com endpoint. Just remember to adjust the URIs to match your own FormAssembly edition.
***

## Registration

Before you can interact with the API, you must obtain a `CLIENT_ID` and a `CLIENT_SECRET` code. This is done by completing the registration process on the particular instance you are targeting. For the Developer Sandbox and FormAssembly.com, you must register through the [FormAssembly Developer Hub](http://www3.formassembly.com/api/). For Enterprise instances, the FormAssembly administrator can register your app: see "[How to Self-Register an Enterprise Application](self-register.md)" for details.

***

## Authorization

**Authorization** is the mechanism that lets FormAssembly users decide which applications can access their account through the API. Authorizations are handled using the [OAuth2](http://oauth.net/2/) protocol.

If you're not familiar with OAuth2 interactions, it can be helpful to look at the documentation of other APIs, [such as this one for the Google API](https://developers.google.com/accounts/docs/OAuth2), explaining how OAuth2 works in detail.

The key point of OAuth2 is that it allows a **user** of FormAssembly (let's call them Adam), to authorize a **client** (you) to take actions in FormAssembly as if you were Adam. For example, you could write an application, which when authorized by Adam, could download and display a list of all of Adam's forms along with associated metrics, e.g., number of responses, dropout rates, etc.  This data could then be used by you to generate custom reports for Adam.

For a step-by-step outline, see the [authorization documentation](authorization.md).


## Example

###  Request

A HTTP GET request to:

https://app.formassembly.com/api_v1/forms/index.json?access_token=ACCESS_TOKEN

The URL is composed of the following parts:

 + `https://app.formassembly.com/`: The FormAssembly instance.
 + `api_v1`: The API version in use. As changes are made to the API, new versions can be released (e.g., under api_v2, api_v3, etc.).
 + `forms/index`: The requested data (see [API Reference](#api-reference) below).
 + `json`: The requested format for the response.
 + `ACCESS_TOKEN`: The token received in [Step 3](#3-request-an-access-token) of the authorization process.


### Response

The response from the request above will result in a JSON-formatted list of the forms in the user's account:

    {
      "Forms":[
        {
          "Form":{
            "id":"XXXX",
            "version_id":"XXXX",
            "name":"XXXXXXXX",
            "category":"XXXX",
            "subcategory":"XXXX",
            "is_template":"XXXX",
            "display_status":"XXXX",
            "moderation_status":"XXXX",
            "expired":"XXXX",
            "use_ssl":"XXXX",
            "user_id":"XXXX",
            "created":"XXXXXXXX",
            "modified":"XXXXXXXX",
            "Aggregate_metadata":{
              "id":"XXXX",
              "response_count":"XXXX",
              "submitted_count":"XXXX",
              "saved_count":"XXXX",
              "unread_count":"XXXX",
              "dropout_rate":"XXXX",
              "average_completion_time":"XXXX",
              "is_uptodate":"XXXX"
            }
          }
        }
      ]
    }

For an explanation of each field, please see [Object Reference](forms.md#object-reference).

Several output formats are accepted. See [Formats](#formats) for more details.

***

## API Reference

### Formats

FormAssembly supports returning data in two main formats:

  + [json](http://en.wikipedia.org/wiki/Json): A lightweight data exchange format supported by newer languages.  Directly parsable by JavaScript.
  + [xml](http://en.wikipedia.org/wiki/XML): An industry standard data exchange format, parsable by almost all languages.

Some endpoints support additional formats, including:
  + [plist](http://en.wikipedia.org/wiki/Plist): Used to provide Apple consumable data for Objective-C applications.
  + [csv](http://en.wikipedia.org/wiki/Comma-separated_values): Used as a standard record data exchange format.
  + [zip](http://en.wikipedia.org/wiki/ZIP_(file_format\)): A binary data container format.

***

### Individual references

* [Forms](forms.md)
* [Responses](responses.md)
