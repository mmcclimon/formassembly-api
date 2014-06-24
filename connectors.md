# Connectors API

The new connectors API is still in development. It does not yet allow for
complete control over the connectors (as in the web interface), but provides
methods for the most common operations. For now, the connectors API only
returns JSON.

Note that unlike the rest of the API, the connectors API allows for the
modification (not just retrieval) of data. The endpoints which modify data
must be submitted as HTTP POST requests, while those that retrieve data are
submitted as GET requests.

## Connector names

The URL scheme for all of the connector API endpoints looks like this:

    [scheme]://[host]/api_v1/connectors/#CONNECTOR_NAME#/...

As with all other API requests, the parameter `access_token` must be provided
as an HTTP GET parameter appended to the endpoint URL.

- `#CONNECTOR_NAME#` is the official name of the connector, one of:
    - `authorize_dot_net`
    - `cybersource`
    - `freshbooks`
    - `googlespreadsheets`
    - `http_post`
    - `ldap_pull`
    - `paypal`
    - `post_redirect`
    - `salesforce2`
    - `salesforce_pull`

## Endpoints

## Connector index

**Endpoint:** `GET /api_v1/connectors/index/$FORM_ID.json`

- List of all controllers for a given form. This will return (among other
  things) the connector ID, which you'll need for interacting with a particular
  connector later.

Return:

    {
      "Connectors": [
        {
          "Connector": {
                "id": int,
                "form_id": int,
                "name": string,
                "status": int (enum, 0 or 1 for disabled/enabled),
                "sequence": int (0-based, order of execution),
                "transaction_template_id": int,
          }
        },
        (other connectors follow)
      ]
    }

Other than the `id`, the `status` key here is probably the most important; `0`
means the connector is disabled, and `1` means that it is enabled.

## View connector

**Endpoint:**  `GET /api_v1/connectors/#CONNECTOR_NAME#/view/#CONNECTOR_ID#.json`

Parameters:

- `#CONNECTOR_NAME#` is one of the connector names, given above
- `#CONNECTOR_ID#` is the ID of this connector, as given in the `index`
  endpoint above.

Return:

    {
        "Connector": {
            "id": int,
            "form_id": int,
            "name": string,
            "remote_uri": string,
            "login": string,
            "mapping": XML string,
            "created": timestamp (YYYY-MM-DD hh:mm:ss),
            "modified": timestamp,
            "transaction_template_id": int,
            "sequence": int (0-based)
            "status": int (enum, 0 or 1),
            "authentication_type": string,
        },
        "Form": {
            "name": string,
            "id": int (matches form_id above),
            "user_id": int,
            "language": string (language code, like "en_US"),
            "use_ssl": int (enum, 0 or 1)
        }
    }


## Edit connector

**Endpoint:**  `POST /api_v1/connectors/#CONNECTOR_NAME#/edit/#CONNECTOR_ID#.json`

Edit an existing connector. URL parameters as in the `view` endpoint

Post fields (all are optional except id and form id, missing fields will be
unchanged):

|name   | value     |
|-------|-----------|
| id    | int, required|
| form_id| int, required|
| remote_uri| string, URL to submit to|
| login|string, username|
| sequence| int, order of execution (0-based)|
| status| int (0 or 1, disabled/enabled)|

Return: As in `view` action above.

## Create connector

**Endpoint:**  `POST /api_v1/connectors/#CONNECTOR_NAME#/create/#FORM_ID#.json`

Create a new connector. `#CONNECTOR_NAME#` parameter as above. Note that this
endpoint requires the *form* ID on which to create the connector, not the
connector ID (it's new, so it will not have an ID yet).

Post fields (all required):

|name       | value     |
|-----------|-----------|
| form_id   | int, required: must match URL form id |
| event     | string, enum |

`event` defines when a particular connector should occur. It must be one of
the following strings:

* `beforerender` - Before a form is rendered (useful for pre-filling form data
  with LDAP information, for example).
* `save` - When a user saves a form to resume later
* `interactive` - Just after a form is submitted, before saving to the
  database. This is the most common option.
* `background` - After form submission, when the user has already seen the
  "Thank you" page.


Return:

    {
        "error": int, enum (0 or 1)
        "message": string|null (string if error, null if success),
        "connector": {
            "id": int,
            "sequence": int,
            "transaction_template_id": int
        }
    }

The `connector` object will be empty if there is an error. Once a user has the
new connector ID, they can then edit it using the `edit` endpoint.

## Delete connector

**Endpoint:**  `POST /api_v1/connectors/#CONNECTOR_NAME#/delete/#CONNECTOR_ID#.json`

Delete an existing connector. URL parameters as above.

Post fields:

    {
        "id": int, required (must match URL id)
    }

Return:

    {
        "success": int, enum (0 or 1)
    }

