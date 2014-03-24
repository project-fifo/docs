---
layout: default
category: jingles
section: jingles/general
title: Jingles General
---
# Jingles General

## Login<a id="login"></a>

![Login](/assets/img/jingles/login.jpg)

When you first connect to the web interface via a web browser you will be presented with a login screen as shown above.

Enter your valid credentials and click the "Ok" button to authenticate and login to the web interface.

<p class="bs-callout bs-callout-info">
The little green dot in the top right corner, pointed to by the red arrow, shows that the web interface is successfully communicating with and connected to the backend. Should the web interface not be connected to the backend the dot will change to red and show an error message.
</p>

## Dashboard<a id="dashboard"></a>

![Dashboard](/assets/img/jingles/dashboard.jpg)

The dashboard is designed to give you a 10 thousand foot view or single pane of glass into your operational cloud. In future versions of FiFo this page may have additional dials and graphs for things such as average cpu usage and disk IO across your cloud as well as possibly show an virtual machines currently being heavily zfs io throttled or nearing its memory RSS limits.

The status page is designed to show you an overview of your entire clouds status.

1. The graphic as shown "green tick" symbol reflects that your cloud is healthy, this image will change to a red symbol should fifo detect a problem such as a compute node becoming unavailable or one of the monitored cloud metrics moving below or above a safety threshold.
2. The "memory bar" shows the percentage of used memory across your entire cloud. Please Note: fifo includes memory allocated for both running and stopped virtual machines in the total percentage.
3. The "disk bar" shows the percentage of disk storage used across your entire cloud.
4. Shows a quick overview of how many virtual machines exist in your cloud as well as the number of hypervisors (compute servers) that make up your cloud.

## Cloudview<a id="cloudview"></a>

![Cloudview](/assets/img/jingles/cloudview01.png)

The CloudView page shows a graphical realtime representation of your entire cloud. This "10,000 foot view" is extremely useful is visualising your cloud in operation as well as quickly determining which vm's or nodes are under load or misbehaving.

1. CloudView Controls
 - Full screen : Expands CloudView to full screen view. Very useful when you have a large cloud with a lot of nodes.
 - Reset : This resets the controls to default.
 - Zoom : Controls the zoom level (The higher the zoom level the more elements you can fit in the same space)
 - Distance :  The display distance of each vm from its central node.
 - Charge : The distance between each SmartOS server node.
 - Elasticity :  How elastic (wiggly) each vm's connection is in relation to its SmartOS node.

2. CloudView icons and symbols
 - ![Hypervisor](/assets/img/jingles/cloudview02.png)
     - Hovering your mouse pointer over a individual compute node will trigger a pop up window showing you information for that specific node.
     - Clicking on a node will take you to the hypervisors details page for that specific node.
     - Each compute node is represented by a "server graphic" and the total amount of physical ram in that node is overlaid on the graphic.
     - A  "memory bar" is shown under each node and shows how much of the total ram in each node is allocated to virtual machines (used, shown in yellow)
     - The name of the node is displayed under the "memory bar"
 - ![VM](/assets/img/jingles/cloudview03.png)
     - Hovering your mouse pointer over a individual machine will trigger a pop up window showing you information for that specific machine.
     - Clicking on a individual machine will take you to the machine details page for that specific machine.
     - A image icon is show for each type of machine. In the above example a SmartOS image is shown for a Joyent branded zone machine and a Penguin image is shown for a Linux KVM machine.
     - Each machine is attached to its host via a dashed connector line. This line will darken and flash bold in realtime representing the network throughput that machine is currently experiencing.
     - Machines that are heavily utilising their allocated cpu will be surrounded by a red circle. The more cpu being consumed, the darker the circle will become.

## About<a id="about"></a>

![about](/assets/img/jingles/about.png)

The about page contains useful or relevant information such as package versions, FiFo team members and contributors as well as a full copy of the open source licence (CDDL) the project is licensed under.

1. Various links: Contains links related to the project:
 - website : The projects website (you are currently on it). http://project-fifo.net/
 - bug tracker : The projects issue tracker where you can log problem tickets. http://jira.project-fifo.net/
 - mailing list : Link to projects google groups mailing list.
 - github : The projects source code repository. https://github.com/project-fifo
  - irc : This launches an additional browser window that connects you to the #project-fifo official irc channel on freenode.
 - follow us : A link to follow the projects official twitter feed. @project_fifo
2. Versions : Shows the current versions of all the FiFo services and indicates whether new package updates are available.
2. Messages : A message section that shows relevent or pertinent messages related to your installation.
