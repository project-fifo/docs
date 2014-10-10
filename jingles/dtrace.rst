.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*******
D-Trace
*******

List
####

.. image:: /_static/images/jingles/dtraces01.png

This section lists all the currently known D-Trace scripts.

	1. Deletes an existing D-Trace script.
	2. Creates a new D-Trace script.

Create
######

.. note::
	
	Right now FiFo only supports running scripts that output `lquantize`.

.. image:: /_static/images/jingles/dtraces02.png

Adding a new script is rather simple, it is a plain D-Trace script however there is an exception, since the script runs on multiple servers and needs to be configurable. Variables are encased in ``$`` signs, and can be chosen freely except one special placeholder: ``$filter$`` is the filter created to match the system it is supposed to run on.


	1. The code of the script, with the variables.
	2. The name the script is shown as.
	3. The variables, they get automatically filled as you type them.
	4. Allows for adding variables ahead of time.
	5. Click to create.


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
