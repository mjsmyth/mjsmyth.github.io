# Reference Abiquo OpenID Connect integration
This page contains the full description of the properties to configure in the Abiquo OpenID integration.

## Table of Abiquo OpenID Connect properties

To enable the OpenID Connect mode, configure the following properties in Abiquo:

| Property | Description |
| :---- | :---- |
| **OpenID server configuration** | |
| `abiquo.auth.module` | The Abiquo authentication module. Must be: `openid` |
| `abiquo.openid.cookie.maxage` | After the OpenID authentication flow, the API redirect adds a cookie with the access token and the id token. The expiry of the OpenID authentication cookie in seconds. A negative value means that the cookie isn't stored persistently and will be deleted when the web browser exits. A zero value causes the cookie to be deleted. *Default: 30* |
| `abiquo.openid.cookie.refreshtoken.include` | If true, the OpenID authentication cookie also contains the refresh token. *Default: false* |
| `abiquo.openid.target` | The URL where the user is redirected from the Identity Server upon successful authentication. Something like `http://<abiquo ui host>/ui/#/dashboard` |
| `abiquo.openid.role-claim` | The claim that identifies the user's role. Abiquo uses its value to map the user to an Abiquo role. |
| `abiquo.openid.enterprise-claim` | The claim that identifies the user's enterprise. Abiquo uses its value to map the user to an Abiquo enterprise. |
| `abiquo.openid.enterprise-property` | Optional: If present, Abiquo searches for an enterprise that has a property with the name configured in this property. It uses the value to match the enterprise claim when resolving the user's enterprise. If absent, Abiquo looks for an enterprise with the name returned in the enterprise claim. |
| `abiquo.openid.issuer` | The OpenID Connect authorization issuer. |
| `abiquo.openid.authorization.endpoint` | The OpenID Connect authorization endpoint. ***This endpoint must be accessible from the user's browser*** |
| `abiquo.openid.token.endpoint` | The OpenID Connect token endpoint. This endpoint must be accessible from the Abiquo server. |
| `abiquo.openid.userinfo.endpoint` | The OpenID Connect user info endpoint. This endpoint must be accessible from the Abiquo server. |
| `abiquo.openid.jwks.endpoint` | The OpenID Connect JWKS endpoint. This endpoint must be accessible from the Abiquo server. |
| `abiquo.openid.endsession.endpoint` | Optional: If configured, Abiquo attempts to perform a global logout performing a request to this endpoint. This is part of the [Session Management](http://openid.net/specs/openid-connect-session-1_0.html) optional spec. ***This endpoint must be accessible from the user's browser.*** |
| **OpenID Connect client configuration** | |
| `abiquo.openid.client.name` | The name of the client on the OpenID Connect server for the Abiquo platform. |
| `abiquo.openid.client.id` | The ID of the client on the OpenID Connect server for the Abiquo platform. |
| `abiquo.openid.client.secret` | The secret of the client on the OpenID Connect server for the Abiquo platform. |
| `abiquo.openid.client.scopes` | Comma separated list of scopes to request during authentication. Must have, at least: `openid,profile,email`. Also supports: `phone`. |
| `abiquo.openid.client.redirect-uris` | Comma separated list of allowed redirect (callback) URIs used during the authentication flow. Must be: `http://<api endpoint>/api/openid_connect_login` |
| `abiquo.openid.client.acr-values` | Space separated values for the acr values to send to OpenID Connect Server when authenticating. They are validated if the `acr-validation` property is true (default value). |
| `abiquo.openid.client.acr-validation` | Activates the acr values validation. Default value is true |

## Reference OpenID Connect client UI properties

Configure the OpenID Connect client UI properties in the `client-config-custom.json` file.

| Property | Description |
| :---- | :---- |
| `client.openid.enabled` | **Deprecated in Abiquo 4.7.1** |
| `client.openid.skip.login.view` | **Deprecated in Abiquo 4.7.1 for UI 5.**. By default, when in OpenID mode, Abiquo shows an initial screen with a link to the Authentication portal. If this property is `true`, then Abiquo doesn't display the initial screen and it redirects users directly to the Authentication portal. |
| `client.skip.login.view` | By default, when in OpenID mode, Abiquo shows an initial screen with a link to the Authentication portal. If this property is `true`, Abiquo doesn't display the initial screen and it redirects users directly to the Authentication portal. |
| `client.auth.module` | Abiquo login modules to use with options for Basic Auth (default), Open ID, and SAML. See `client-config-default.json` for examples. |
