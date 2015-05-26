.. Project-FiFo documentation master file, created by
   Mark Slatem on 7th May 2015.

LeoFS
#######

`LeoFS`_ is what Project FiFo uses for its object storage. LeoFS is an unstructured object store for the web. It is highly available, distributed, eventually consistent scalable storage system. LeoFS supports a huge amount of various kinds of unstructured data and is `S3_API`_ compatible. Project FiFo use LeoFS for storing VM datasets, snapshots and backups.

Project FiFo provides all the required packages needed to get the LeoFS services running within SmartOS Zones. This makes it easy to get your underlying LeoFS storage in place prior to installing FiFo. A fully functional LeoFS storage system is **required** for Project-FiFo to work.  

.. _LeoFS: http://leo-project.net/leofs/
.. _S3_API: http://docs.aws.amazon.com/AmazonS3/latest/API/APIRest.html


.. toctree::
   :maxdepth: 2
   :glob:

   leofs/overview
   leofs/installation
