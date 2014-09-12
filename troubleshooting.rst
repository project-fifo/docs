.. Project-FiFo documentation master file, created by
  Kevin M. Meziere on Sat Aug 30 11:16:33 2014.

Problem Checklist
##################

Since we often encounter the same kind of problems here is a checklist how to track them down most easiely.

First some general information you'll need:

 
+-----------------+---------------------------------+
| Info            | Command                         |
+=================+=================================+
| Hypervisor IP	  | ``ifconfig``                    |
+-----------------+---------------------------------+
| Zone IP   	  | ``ifconfig``                    |
+-----------------+---------------------------------+
| Wiggle Version  | ``pkgin list | grep wiggle``    |
+-----------------+---------------------------------+
| Sniffle Version | ``pkgin list | grep sniffle``   |
+-----------------+---------------------------------+
| Howl Version    | ``pkgin list | grep howl``      |
+-----------------+---------------------------------+
| Snarl Version   | ``pkgin list | grep snarl``     |
+-----------------+---------------------------------+


+----------------------------------------------+
|Log Locations                                 |
+-------------+--------+-----------------------+
| Component   | GZ     | Directory             |
+=============+========+=======================+
| Chunter     | GZ     | /var/log/chunter      |
+-------------+--------+-----------------------+
| Sniffle     | GZ     | /var/log/sniffle      |
+-------------+--------+-----------------------+
| Snarl       | GZ     | /var/log/snarl        |
+-------------+--------+-----------------------+
| Howl        | GZ     | /var/log/howl         |
+-------------+--------+-----------------------+
| Wiggle      | GZ     | /var/log/wiggle       |
+-------------+--------+-----------------------+

UI Related
----------

If you have UI related problems please try first to clear the cookies for the fifo page.


General Checks
--------------

- **FiFo zone is running**

    **Run location: GZ**
    Command (example):

    .. code-block:: bash

        vmamd list | grep fifo
        cae70926-cdc8-430f-9138-e06ac596c3ad  OS    512      running           fifo

____

- **All processes are running**

    **Run location: GZ**
    Command (example):

    .. code-block:: bash

        ps -afe | grep run_erl | grep -v grep
        root 48037 1 0 Dec 27 ? 0:00 /opt/chunter/erts-5.9.1/bin/run_erl -daemon /tmp/chunter /var/log/chunter exec 
        0000105 65606 1 0 10:38:57 ? 0:00 /opt/local/wiggle/erts-5.9.1/bin/run_erl -daemon /tmp/wiggle/ /var/log/wiggle e
        0000104 99374 1 0 08:39:13 ? 0:00 /opt/local/sniffle/erts-5.9.1/bin/run_erl -daemon /tmp/sniffle/ /var/log/sniffl
        0000103 42238 1 0 06:55:02 ? 0:00 /opt/local/snarl/erts-5.9.1/bin/run_erl -daemon /tmp/snarl/ /var/log/snarl exec
        0000106 56504 1 0 07:20:50 ? 0:00 /opt/local/howl/erts-5.9.1/bin/run_erl -daemon /tmp/howl /var/log/howl exec /op

____

- **Chunter service is running**

    **Run location: GZ**
    Command (example):

    .. code-block:: bash

        svcs chunter
        STATE  STIME  FMRI
        online Jan_27 svc:/network/chunter:default

____

- **Communication between GZ and FiFo Services**

    **Run location: GZ**
    Command (example):

    .. code-block:: bash

        ping $FIFOZONEIP
        192.168.0.126 is alive

____

- **DNS reaches the hypervisor**

    **Run location: GZ**
    Command (example):

    *might take ~2 minutes*

    .. code-block:: bash

        snoop inet 224.0.0.251
        172.16.234.10 -> 224.0.0.251 MDNS R _snarl._tcp.local. Internet PTR build._snarl._tcp.local.
        172.16.234.10 -> 224.0.0.251 MDNS R _howl._tcp.local. Internet PTR build._howl._tcp.local.
        172.16.234.10 -> 224.0.0.251 MDNS R _sniffle._tcp.local. Internet PTR build._sniffle._tcp.local.

____

- **Chunter's listening IP**

    **Run location: GZ**
    Command (example):

    .. code-block:: bash

        grep ip /opt/chunter/etc/chunter.conf
        ip = <ip in the same network as the fifo zone>:4200

____

- **FiFo zone has network connectivity**

    **Run location: FiFo Zone**
    Command (example):

    .. code-block:: bash

       ping project-fifo.net
       project-fifo.net is alive

____

- **All FiFo services are running**

    **Run location: FiFo Zone**
    Command (example):

    .. code-block:: bash

       svcs wiggle sniffle snarl howl
       STATE  STIME  FMRI
       online Jan_25 svc:/network/snarl:default
       online Jan_25 svc:/network/howl:default
       online Jan_25 svc:/network/sniffle:default
       online Jan_27 svc:/network/wiggle:default

____

- **Memory is scaled correctly**

    **Run location: FiFo Zone**
    Command (example):

    .. code-block:: bash

       prstat
          PID USERNAME  SIZE   RSS STATE  PRI NICE      TIME  CPU PROCESS/NLWP
        27833 howl      109M   54M sleep   56    0   2:06:24 0.3% beam.smp/164
        27916 wiggle     91M   54M sleep   54    0   1:38:29 0.2% beam.smp/77
        27846 snarl     358M   61M sleep   57    0   0:29:30 0.1% beam.smp/172
        27841 sniffle  1112M  771M sleep   56    0   1:39:02 0.1% beam.smp/184

    *Note that SIZE is much bigger then RSS, this is caused by mmaped files for the database and can cause problems if it grows too big!*

____

 
Problems with the API
---------------------

- **Manually try to login**

    **Run location: Client**
    Command (example):

    .. code-block:: bash

        curl -v "http://<IP>/api/0.1.0/sessions" -H "Content-Type: application/json" -H "Accept: application/json" --data-binary '{"user":"admin","password":"admin"}'
        * About to connect() to 192.168.0.204 port 80 (#0)
        * Trying 192.168.0.204...
        * connected
        * Connected to 192.168.0.204 (192.168.0.204) port 80 (#0)
        > POST /api/0.1.0/sessions HTTP/1.1
        > User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8r zlib/1.2.5
        > Host: 192.168.0.204
        > Content-Type: application/json
        > Accept: application/json
        > Content-Length: 33
        >
        * upload completely sent off: 33 out of 33 bytes
        < HTTP/1.1 303 See Other
        < Server: nginx/1.3.13
        < Date: Wed, 29 May 2013 10:37:59 GMT
        < Content-Type: application/json
        < Content-Length: 0
        < Location: http://<IP>/api/0.1.0/sessions/64bbb7cb-7505-4b01-adf7-c7daf5b5186a
        < Connection: keep-alive
        < access-control-allow-origin: *
        < access-control-allow-headers: content-type, x-snarl-token
        < access-control-expose-headers: x-snarl-token
        < allow-access-control-credentials: true
        < vary: accept
        < set-cookie: x-snarl-token=64bbb7cb-7505-4b01-adf7-c7daf5b5186a; Version=1; Expires=Wed, 28-May-2014 10:37:59 GMT; Max-Age=31449600
        < x-snarl-token: 64bbb7cb-7505-4b01-adf7-c7daf5b5186a
        <
        * Connection #0 to host 192.168.0.204 left intact
        * Closing connection #0

____

- **Wiggle can connect to Sniffle**

    **Run location: FiFo Zone**
    Command (example):

    .. code-block:: bash

        /opt/local/wiggle/bin/wiggle attach
        Attaching to /tmp/wiggle/erlang.pipe.1 (^D to exit)
        ^R
        libsniffle:servers().
        [{{"fifo.local",[{port,<<"4210">>},{ip,<<"192.168.0.123">>}]},"192.168.0.123",4210}]
        (wiggle@192.168.0.123)2> libsniffle:version(). 
        <<"test-19442d0, Sun Dec 30 08:49:03 2012 +0100">>
        (wiggle@192.168.0.123)3> ^D [Quit]


    Note: ``libsniffle:servers().`` and ``libsniffle:version().`` need to be entered after attaching to wigle

____

- **Wiggle can connect to Snarl**

    **Run location: FiFo Zone**
    Command (example):

    .. code-block:: bash

        /opt/local/wiggle/bin/wiggle attach
        Attaching to /tmp/wiggle/erlang.pipe.1 (^D to exit)
        ^R
        libsnarl:servers().
        [{{"build.local",[{port,<<"4200">>},{ip,<<"192.168.0.123">>}]},"192.168.0.123",4200}]
        (wiggle@192.168.0.123)2> libsnarl:version().
        <<"test-26da855, Sun Dec 30 07:34:20 2012 +0100">>
        (wiggle@192.168.0.123)3> ^D [Quit]


    Note: ``libsnarl:servers().`` and ``libsnarl:version().`` need to be entered after attaching to wigle

____

- **Wiggle can connect to Howl**

    **Run location: FiFo Zone**
    Command (example):

    .. code-block:: bash

        /opt/local/wiggle/bin/wiggle attach
        Attaching to /tmp/wiggle/erlang.pipe.1 (^D to exit)
        ^R
        libhowl:servers().
        [{{"build.local",[{port,<<"4240">>},{ip,<<"192.168.0.123">>}]},"192.168.0.123",4240}]
        (wiggle@192.168.0.123)2> libhowl:version().
        <<"test-26da855, Sun Dec 30 07:34:20 2012 +0100">>
        (wiggle@192.168.0.123)3> ^D [Quit]


    Note: ``libhowl:servers().`` and ``libhowl:version().`` need to be entered after attaching to wigle

____

- **Logs**

    **Run location: FiFo Zone**
    Command (example):

    .. code-block:: bash

        ls -l /var/logs/wiggle
        -rw-r--r-- 1 wiggle wiggle   3225 Jan 27 22:38 console.log
        -rw-r--r-- 1 wiggle www         0 Jan 27 22:38 crash.log.1
                -rw-r--r-- 1 wiggle wiggle 105462 Jan 27 22:40 debug.log
        -rw-r--r-- 1 wiggle www     84890 Jan 29 18:13 erlang.log.1
        -rw-r--r-- 1 wiggle wiggle   1874 Jan 25 18:42 error.log
        -rw-r--r-- 1 wiggle wiggle    270 Jan 27 22:38 run_erl.log
        drwxr-xr-x 2 wiggle wiggle      7 Jan 27 22:38 sasl
        -rw-r--r-- 1 wiggle wiggle   1874 Jan 25 18:42 warning.log
        cat <accordingly> 


    Note: Note crashes can happen even when the system runs fine. 

____

VMs / Hypervisors / etc
-----------------------

- **List hypervisors**

    **Run location: FiFo Zone**
    Command (example):

    .. code-block:: bash

        fifoadm hypervisors list
        Hypervisor         IP               Memory          State
        ------------------ ---------------- --------------- -------------
        00-15-17-b8-16-fc  172.16.0.4       25064/32699     ok

____

- **List VMs**

    **Run location: FiFo Zone**
    Command (example):

    .. code-block:: bash

        fifoadm vms list
        List of VMs	zone	fifoadm vms list	
        UUID                                 Hypervisor        Name            State
        ------------------------------------ ----------------- --------------- ----------
        7df22c41-bade-4b26-b20e-ee2b45e81bf8 00-15-17-b8-16-fc fifo            running 
        21e0bc5d-af4e-4a44-8137-c7d50870dcbd 00-15-17-b8-16-fc ngnix           running 
        bf57045f-42ee-42b5-8dc5-201250b7b6f4 00-15-17-b8-16-fc confluence      running 
        39cd0a98-5087-472c-b89c-e75aef378a22 00-15-17-b8-16-fc dev             stopped 
        49314fda-fef0-42fa-b974-77d27b097aa1 00-15-17-b8-16-fc korny           running 
        2362ebf6-4988-4cfd-89ec-004dcc61a63b 00-15-17-b8-16-fc zotonic         stopped 
        87cc64b1-3990-4cf6-a54d-dbc2e66adddc 00-15-17-b8-16-fc -               installing
        1df09840-f2bb-48fb-a3b3-5fe679849baf 00-15-17-b8-16-fc mail            running 
        6d4a35a6-41d8-4a44-9977-e010b3ed307a 00-15-17-b8-16-fc test            running

____

- **Fetch details on a misbehaving VM**

    **Run location: FiFo Zone**
    Command (example):

    .. code-block:: bash

        fifoadm vms get -j <uuid>
        {
         "hypervisor": "00-15-17-b8-16-fc",
         "state": "installing_dataset"
        }

____

There are a lot more calls for fifoadm that can help depending on where things lead. 


Reporting an issue
-------------------

`JIRA <https://jira.project-fifo.net/>`_ is the best place to file a report. If you do so it is often helpful to attach some logs. They can be found in the ``/var/logs/{sniffle,snarl,howl,wiggle}`` and ``/var/log/chunter`` (in the FiFo Zone or GZ respectively).



