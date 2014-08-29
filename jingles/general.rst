.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*******
General
*******

Login
#####

.. warning::
     
     The OTP (One Time Password) field can be left empty unless a YubiKey was specifically added to the user. This is never the case for newly created users!

.. image:: /_static/images/jingles/login.jpg

When you first connect to the web interface via a web browser you will be presented with a *login screen* as shown above.

Enter your valid credentials and click the "**Ok**" button to authenticate and login to the web interface.

.. note::

     The little green dot in the top right corner pointed to by the red arrow shows that the web interface is successfully communicating with and connected to the backend. Should the web interface not be connected to the backend the dot will change to red and show an error message.

Dashboard
#########

.. image:: /_static/images/jingles/dashboard.jpg

The *Dashboard* is designed to give you a "*10,000 foot view*" or single pane of glass into your operational cloud. In future versions of *FiFo* this page may have additional dials and graphs for things such as average CPU usage and disk IO across your cloud as well as the possibility to show that a virtual machine is currently being heavily zfs io throttled or that it's nearing its memory RSS limits.

The status page is designed to show you an overview of your entire clouds status.

1. The "green tick" symbol as shown in the graphic reflects that your cloud is healthy, this image will change to a red symbol should *FiFo* detect a problem such as a compute node becoming unavailable or one of the monitored cloud metrics moving below or above a safety threshold.
2. The "memory bar" shows the percentage of used memory across your entire cloud. Please Note: *FiFo* includes memory allocated for both running and stopped virtual machines in the total percentage.
3. The "disk bar" shows the percentage of disk storage used across your entire cloud.
4. Shows a quick overview of how many virtual machines exist in your cloud as well as the number of hypervisors (compute servers) that make up your cloud.

Cloudview
#########

.. image:: /_static/images/jingles/cloudview01.png

The *CloudView* page shows a graphical realtime representation of your entire cloud. This "*10,000 foot view*" is extremely useful for visualising your cloud in operation as well as quickly determining which VM's or nodes are under load or misbehaving.

1. CloudView Controls

 * Full screen : Expands CloudView to full screen view. This is very useful when you have a large cloud with a lot of nodes.
 * Reset : This resets the controls to default.
 * Zoom : Controls the zoom level (The higher the zoom level the more elements you can fit in the same space)
 * Distance : The display distance of each VM from its central node.
 * Charge : The distance between each SmartOS server node.
 * Elasticity : How elastic (wiggly) each vm's connection is in relation to its SmartOS node.

2. CloudView icons and symbols

 .. image:: /_static/images/jingles/cloudview02.png

* Hovering your mouse pointer over an individual compute node will trigger a pop up window showing you information on that specific node.
* Clicking on a node will take you to the hypervisors details page for that specific node.
* Each compute node is represented by a "server graphic" and the total amount of physical ram in that node is overlaid on the graphic.
* A  "memory bar" is shown under each node that shows how much of the total ram in each node is allocated to virtual machines (used, shown in yellow)
* The name of the node is displayed under the "memory bar"

 .. image:: /_static/images/jingles/cloudview03.png

* Hovering your mouse pointer over an individual machine will trigger a pop up window showing you information for that specific machine.
* Clicking on an individual machine will take you to the machine details page for that specific machine.
* An image icon is shown for each type of machine. In the above example a *SmartOS* image is shown for a *Joyent branded zone machine* and a Penguin image is shown for a *Linux KVM machine*.
* Each machine is attached to its host via a dashed connector line. This line will darken and flash bold in realtime representing the network throughput that machine is currently experiencing.
* Machines that are heavily utilising their allocated CPU will be surrounded by a red circle. The more CPU is being used, the darker the circle will become.

About
#####

.. image:: /_static/images/jingles/about.png

The *About Page* contains useful and relevant information such as package versions, *FiFo* team members and contributors as well as a full copy of the open source licence (CDDL) the project is licensed under.

1. Various links : Contains links related to the project:

 * website : The project's website (you are currently on it). http://project-fifo.net/
 * bug tracker : The project's issue tracker where you can log problem tickets. http://jira.project-fifo.net/
 * mailing list : Link to the project's google groups mailing list.
 * github : The project's source code repository. https://github.com/project-fifo
 * irc : This launches an additional browser window that connects you to the #project-fifo official irc channel on freenode.
 * follow us : A link to follow the project's official twitter feed. @project_fifo
 
2. Versions : Shows the current versions of all the *FiFo* services and indicates whether new package updates are available.

3. Messages : A message section that shows relevent or pertinent messages related to your installation.
