.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
API - Sessions
**************

.. http:post:: /sessions

   Logs a user in.

   **Version**

    *0.1.0* use the OAuth2.0 mechanism for 0.2.0 and following.

   **Related permissions**

    *not needed*

   **Example request**:

   .. sourcecode:: http

	  POST /sessions HTTP/1.1
	  host: cloud.project-fifo.net
	  accept: application/json
	  content-type: application/json

	  {
	   "user": "admin",
	   "password": "admin"
	  }

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 302 Found
      location: /sessions/1b2230af-03bb-4bf7-ab49-86fab503bf16

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader content-type: datatype used in the body, usually ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 302: redirect to the session :http:get:`/sessions/(token:session)`
   :status 400: body missing user or password field
   :status 403: the provided information is invalid for login
   :status 449: the user requires MFA but no ``otp`` parameter was passed
   :status 503: one or more subsystems could not be reached

   :<json string user: user to log in
   :<json string password: password to log in with
   :<json string otp: YubiKey OTP *(optional)*

____


.. http:get:: /sessions/(token:session)

   Retrives session data, analogous to `GET /users/UUID <users.html#get--users-(uuid-%20user)>`_

   **Version**

    *0.1.0* use :http:get:`/sessions` in 0.2.0 with OAuth2.0

   **Related permissions**:

    * Only available for current session

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the session information is returned
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached

   :>json string session: Token of the session id for this session

.. http:get:: /sessions/(uuid:session)

   Retrives session data, analogous to `GET /users/UUID <users.html#get--users-(uuid-%20user)>`_

   **Version**

    *0.1.0* use :http:get:`/sessions` in 0.2.0 with OAuth2.0

   **Related permissions**:

    * Only available for current session


   :reqheader accept: the accepted encoding, valid is ``application/json``
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the session information is returned
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached

____


.. http:get:: /sessions

   Retrives session data, analogous to `GET /users/UUID <users.html#get--users-(uuid-%20user)>`_

   **Version**

    *0.2.0*

   **Related permissions**

    * Only available for current session

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :resheader content-type: the returned datatype, usually ``application/json``
   :reqheader authorization: A Bearer token.

   :status 200: the session information is returned
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached

____

.. http:get:: /sessions/one_time_token

   Generates a one time token to be used when opening websocket connections.

   **Version**

    *0.2.0*

   **Related permissions**

    * Only available for current session

   :resheader content-type: the returned datatype, usually ``application/json``
   :reqheader authorization: A Bearer token.

   :status 200: the session information is returned
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached

   :>json string token: The one time token.
   :>json integer expiery: The amount of seconds the token is valid, defaults to 30.

____

.. http:delete:: /sessions/(uuid:session)

   Deletes the session with the given `token`, logging it out.

   **Example request**:

   .. sourcecode:: http

     DELETE /sessions/1b2230af-03bb-4bf7-ab49-86fab503bf16 HTTP/1.1
     host: cloud.project-fifo.net
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the session was successfully deleted
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached

