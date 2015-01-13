.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*******
D-Trace
*******

To get to *Fifo's* DTrace management section: 

1. Click **More** on the home screen and than select **Configuration**.

2. On the next page select **Networking**

.. image:: /_static/images/jingles/dtraces_getto.png

List
####

.. image:: /_static/images/jingles/dtraces01.png

This section lists all the currently known D-Trace scripts.

1. By clicking the |new network| button you can create a new DTrace.
2. By clicking the |delete network| button next to the DTrace you can delete an existing DTrace.

.. |new network| image:: /_static/images/jingles/dtraces-new.png
.. |delete network| image:: /_static/images/jingles/networks-delete.png


Create
######

.. note::
	
	Right now FiFo only supports running scripts that output `lquantize`.

.. image:: /_static/images/jingles/dtraces02.png

Adding a new script is rather simple. It is a plain D-Trace script, however there is an exeptation since the script runs on multiple servers and needs to be configurable. Varlables are encased in ``$`` signes and can be choosen freely. The only exception is the special placeholder: ``$filter$``. It is the filter created to match the system it is supposed to run on.

	1. Enter the name of the new script.
	2. Enter the code of the script with the variables.
	3. Enter variables needed ahead of time.
	4. By clicking the |create button| button you can create the new DTrace.

.. |create button| image:: /_static/images/jingles/create.png

Details
#######

.. image:: /_static/images/jingles/dtraces03.png

The details view lets you see the script in it's raw form and the default variables it has. However it can not be changed.

.. image:: /_static/images/jingles/dtraces04.png

This section allows you to run a D-Trace script either against individual VM's or Servers. If a VM is selected there is no need to select the servers they are on, FiFo will calculate that automatically.

	1. The Server to run upon.
	2. The VM's to run against.
	3. This section will show a heatmap of the output.
	4. The variables to run the script with, this can be adjusted to fit the current usecase.
