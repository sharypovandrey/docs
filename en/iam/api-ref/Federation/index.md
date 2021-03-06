---
editable: false
---

# Federation
A set of methods for managing federations.
## JSON Representation {#representation}
```json 
{
  "id": "string",
  "folderId": "string",
  "name": "string",
  "description": "string",
  "createdAt": "string",
  "cookieMaxAge": "string",
  "autoCreateAccountOnLogin": true,
  "issuer": "string",
  "ssoBinding": "string",
  "ssoUrl": "string"
}
```
 
Field | Description
--- | ---
id | **string**<br><p>Required. ID of the federation.</p> <p>The maximum string length in characters is 50.</p> 
folderId | **string**<br><p>Required. ID of the folder that the federation belongs to.</p> <p>The maximum string length in characters is 50.</p> 
name | **string**<br><p>Required. Name of the federation.</p> <p>Value must match the regular expression <code>\|[a-z][-a-z0-9]{1,61}[a-z0-9]</code>.</p> 
description | **string**<br><p>Description of the federation.</p> <p>The maximum string length in characters is 256.</p> 
createdAt | **string** (date-time)<br><p>Creation timestamp.</p> <p>String in <a href="https://www.ietf.org/rfc/rfc3339.txt">RFC3339</a> text format.</p> 
cookieMaxAge | **string**<br><p>Browser cookie lifetime in seconds. If the cookie is still valid, the management console authenticates the user immediately and redirects them to the home page.</p> <p>Acceptable values are 600 seconds to 43200 seconds, inclusive.</p> 
autoCreateAccountOnLogin | **boolean** (boolean)<br><p>Add new users automatically on successful authentication. The user will get the <code>resource-manager.clouds.member</code> role automatically, but you need to grant other roles to them.</p> <p>If the value is <code>false</code>, users who aren't added to the cloud can't log in, even if they have authenticated on your server.</p> 
issuer | **string**<br><p>Required. ID of the IdP server to be used for authentication. The IdP server also responds to IAM with this ID after the user authenticates.</p> <p>The maximum string length in characters is 8000.</p> 
ssoBinding | **string**<br><p>Single sign-on endpoint binding type. Most Identity Providers support the <code>POST</code> binding type.</p> <p>SAML Binding is a mapping of a SAML protocol message onto standard messaging formats and/or communications protocols.</p> <ul> <li>POST: HTTP POST binding.</li> <li>REDIRECT: HTTP redirect binding.</li> <li>ARTIFACT: HTTP artifact binding.</li> </ul> 
ssoUrl | **string**<br><p>Required. Single sign-on endpoint URL. Specify the link to the IdP login page here.</p> <p>The maximum string length in characters is 8000.</p> 

## Methods {#methods}
Method | Description
--- | ---
[addUserAccounts](addUserAccounts.md) | Adds users to the specified federation.
[create](create.md) | Creates a federation in the specified folder.
[delete](delete.md) | Deletes the specified federation.
[get](get.md) | Returns the specified federation.
[list](list.md) | Retrieves the list of federations in the specified folder.
[listOperations](listOperations.md) | Lists operations for the specified federation.
[update](update.md) | Updates the specified federation.