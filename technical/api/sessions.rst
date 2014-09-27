.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
API - Sessions
**************

.. http:post:: /sessions

   Logs a user in.

   **Related permissions**

    *not needed*

   **Example request**:

   .. sourcecode:: http

	  POST /session HTTP/1.1
	  host: cloud.project-fifo.net
	  accept: applicaiton/json
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

   :status 302: redirect to the session :http:get:`/sessions/(uuid:session)`
   :status 400: body missing user or password field
   :status 403: the provided information is invalid for login
   :status 449: the user requres MFA but no ``otp`` parameter was passed
   :status 503: one or more subsystems could not be reached

   :<json string user: user to log in
   :<json string password: password to log in with
   :<json string otp: YubiKey OTP *(optional)*

____


.. http:get:: /sessions/(uuid:session)

   Retrives session data, analogous to `GET /users/UUID <users.html#get--users-(uuid-%20user)>`_

   **Related permissions**

    *not needed*

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the session information is returned
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached

   :>json session name: UUID of the session id for this session

   **Related permissions**:

    * Only available for current session

____

.. http:delete:: /sessions/(uuid:session)

   Deletes the session with the given `uuid`, logging it out.

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

