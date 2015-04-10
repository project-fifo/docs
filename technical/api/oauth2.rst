.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

************
API - OAuth2
************

.. http:get:: /oauth2/auth

   Shows the OAuth2 request form to authorize a client for a given scope

   **Version**

    *0.2.0*

   **Related permissions**

    *not needed*

   **Example request**:

   .. sourcecode:: http

	  GET /oauth2/auth HTTP/1.1
	  host: cloud.project-fifo.net

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 200 OK

      <html>...</html>

   :query response_type: One of `code` and `token`
   :query client_id: ID of the client
   :query state: State of the client.
   :query redirect_uri: The URI redirected to at the end of the OAuth2 dacnce.
   :query scope: The requested scope.

   :reqheader authorization: A Bearer token or basic auth user/password pair. In that case only a confirmation is requested no login required.

   :status 200: Shows the form.

____

.. http:post:: /oauth2/auth

   Grants a `access token` or `authroization code` request and redirects on. On completion might redirect to :http:get:`/oauth2/2fa` to perform a second set of two factor autentication if required.

   **Version**

    *0.2.0*

   **Related permissions**

    *not needed*

   **Example request**:

   .. sourcecode:: http

      POST /oauth2/auth HTTP/1.1
	  host: cloud.project-fifo.net
	  content-type: application/x-www-form-urlencoded

      response_type=code&client_id=Client1&redirect_uri=http://client.uri&state=foo&username=User&password=Pass

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 302 Found
      location: http://client.uri?access_code=abaADbAD123BADas


   :form response_type: One of `code` and `token`
   :form client_id: ID of the client
   :form state: State of the client.
   :form redirect_uri: The URI redirected to at the end of the OAuth2 dacnce.
   :form scope: The requested scope.
   :form username: The username to authenticate with. (optional if basic auth or bearer tokens are used0
   :form password: The password to authenticate with.

   :reqheader authorization: A Bearer token or basic auth pair in place of `username` and `password`.

   :status 302: Redirect to either the client or the 2fa form.

____

.. http:get:: /oauth2/2fa

   Shows the OAuth2 Two Factor Authenticatin (2fa) form

   **Version**

    *0.2.0*

   **Related permissions**

    *not needed*

   **Example request**:

   .. sourcecode:: http

	  GET /oauth2/2fa HTTP/1.1
	  host: cloud.project-fifo.net

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 200 OK

      <html>...</html>

   :query response_type: One of `code` and `token`.
   :query redirect_uri: The URI redirected to at the end of the OAuth2 dacnce.
   :query state: State of the client.
   :query fifo_otp_token: Then OTP token that is used to associate the OTP response to a ongoing request.

   :status 200: Shows the form.

____

.. http:post:: /oauth2/2fa

   Grants a `access token` or `authroization code` request and redirects on.

   **Version**

    *0.2.0*

   **Related permissions**

    *not needed*

   **Example request**:

   .. sourcecode:: http

      POST /oauth2/2fa HTTP/1.1
	  host: cloud.project-fifo.net
	  content-type: application/x-www-form-urlencoded

      response_type=code&fifo_otp_token=abaADbAD123BADas&redirect_uri=http://client.uri&state=foo&fifo_otp

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 302 Found
      location: http://client.uri?access_code=abaADbAD123BADas


   :form response_type: One of `code` and `token`.
   :form redirect_uri: The URI redirected to at the end of the OAuth2 dacnce.
   :form state: State of the client.
   :form fifo_otp_token: Then OTP token that is used to associate the OTP response to a ongoing request.
   :form fifo_otp: The OTP from the user.

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: A Bearer token or basic auth pair in place of `username` and `password`.

   :status 302: Redirect to either the client.

____

.. http:post:: /oauth2/token

   Handles the following parts of the RFC:
    * `4.1.3 Access Token Request <https://tools.ietf.org/html/rfc6749#section-4.1.3>`_ - `authorization_code`
    * `4.3 Resource Owner Password Credentials Grant <https://tools.ietf.org/html/rfc6749#section-4.3>`_ - (grant type `password`)
    * `4.4.2 Access Token Request <https://tools.ietf.org/html/rfc6749#section-4.4.2>`_ - (grant type `client_credentials`)
    * `6 Refreshing an Access Token <https://tools.ietf.org/html/rfc6749#section-6>`_ - (grant type `refresh_token`)

   **Version**

    *0.2.0*

   **Related permissions**

    *not needed*

   **Example request**:

   .. sourcecode:: http

      POST /oauth2/token HTTP/1.1
	  host: cloud.project-fifo.net
	  content-type: application/x-www-form-urlencoded

      response_type=code&fifo_otp_token=abaADbAD123BADas&redirect_uri=http://client.uri&state=foo&fifo_otp

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 200 OK

      {json object}

   :form grant_type: One of `authorization_code`, `client_credentials`, `refresh_token`, or `password`
   :form code: The URI redirected to at the end of the OAuth2 dacnce. (`authorization_code`)
   :form client_id: Client ID (may be supplied over basic auth) (`authorization_code`, `client_credentials`, `refresh_token`)
   :form client_secret: The OTP from the user. (may be supplied over basic auth) (`authorization_code`, `client_credentials`, `refresh_token`)
   :form redirect_uri: Then OTP token that is used to associate the OTP response to a ongoing request. (`authorization_code`)
   :form refresh_token: Used to obtain a new token via a `refresh_token`.

   :form username: The username for the password grant. (`passwort`)
   :form password: The password for the password grant. (`passwort`)
   :form scope: The Scope for a password grant. (`passwort`)
   :form fifo_otp: The OTP from the user if required.  (`passwort` - custom extension)

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Basic auth to authentiate the client.
   :resheader accept: the accepted encoding, valid is ``application/json``

   :status 200: Reply to the requeest.
