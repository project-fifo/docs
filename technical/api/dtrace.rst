.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

************
API - DTrace
************

.. http:get:: /dtrace

   Lists all dtrace scripts.

   **Related permissions**

      cloud -> dtrace -> list

   **Example request**:

   .. sourcecode:: http

     GET /dtrace HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json

     ["b7c658e0-2ddb-46dd-8973-4a59ffc9957e"]


   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader x-full-list: true - to get a full list instead of UUIDs
   :reqheader x-full-list-fields: fields to include in the full list - please see: :http:get:`/dtrace/(uuid:dtrace)`
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the DTrace list is returned
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

____


.. http:post:: /dtrace

   Creates a dtrace script.

   **Related permissions**

      cloud -> dtrace -> create

    **Example request**:

   .. sourcecode:: http

      POST /api/0.1.0/dtrace HTTP/1.1
      Accept: application/json
      Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6
      Content-Type: application/json

      {
      "config": {"start":127, "end":0, "step":4},
      "name": "erlang function calls",
      "script": "erlang*:::global-function-entry\n$filter$\n{\n  self->t[copyinstr(arg1)] = vtimestamp;\n}\nerlang*:::function-return\n/self->t[copyinstr(arg1)]/\n{\n  @time[copyinstr(arg1)] = lquantize((vtimestamp - self->t[copyinstr(arg1)] ) / 1000, $start$, $end$, $step$);\n  self->t[copyinstr(arg1)] = 0;\n}"
      }

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 303 See Other
      Content-Type: application/json
      vary: accept
      location: /api/0.1.0/dtrace/e864e552-0208-40f9-b05e-6de66f3b3579

   .. todo::

      Add header fields

____


.. http:get:: /dtrace/(uuid:dtrace)

   Returns dtrace script for given *uuid*.

   **Related permissions**

      dtrace -> UUID -> get

   **Example request**:

   .. sourcecode:: http

     GET /dtrace/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

       HTTP/1.1 200 OK
       vary: Accept
       content-type: application/json

       {
        "uuid": "b7c658e0-2ddb-46dd-8973-4a59ffc9957e",
        "name": "zfs reads",
        "script": "/*some dtrace here/*",
        "config": {"start": 0, "end": 64, "step":2},
        "metadata": {}
       }


   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the DTrace information is returned
   :status 403: user is not authorized
   :status 404: the DTrace was not found
   :status 503: one or more subsystems could not be reached

   :>json string UUID: UUID of DTrace
   :>json string name: name of DTrace
   :>json string script: DTrace scirpt
   :>json object config: DTrace config
   :>json object metadata: metadata associated with DTrace

____


.. http:put:: /dtrace/(uuid:dtrace)

   Edits dtrace script with given *uuid*.

   **Related permissions**

      dtrace -> UUID -> edit

   .. warning::

      not currently supported

____


.. http:delete:: /dtrace/(uuid:dtrace)

   Deletes dtrace script with given *uuid*.

   **Related permissions**

      dtrace -> UUID -> delete

   **Example request**:

   .. sourcecode:: http

     DELETE /dtrace/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the DTrace was successfully deleted
   :status 404: the DTrace was not found
   :status 503: one or more subsystems could not be reached

____


.. http:put:: /dtrace/(uuid:dtrace)/metadata[/...]

   Sets a metadata key for dtrace with given *uuid*.

   **Related permissions**

      dtrace -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/metadata/jingles HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6
     Content-Type: application/json

     {"notes":  [{"text":"yap","created_at":"2014-09-13T01:34:03.379Z"}]}

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     vary: accept

   :reqheader accept: the accepted encoding, alis is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the provided datatype, usually ``application/json``

   :status 204: no content
   :status 404: the VM could not be found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json string <key>: values to store under this key



____


.. http:delete:: /dtrace/(uuid:dtrace)/metadata/...

   Removes a metadata key for dtrace with given *uuid*.

   **Related permissions**

     dtrace -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /dtrace/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/(path:metadata) HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the metadata key was successfully deleted from DTrace
   :status 404: the metadata key was not found
   :status 503: one or more subsystems could not be reached
