---
layout: default
category: jingles
section: jingles/dtrace
title: Jingles D-Trace
---
# Jingles D-Trace

### List<a id="dtraces-list"></a>
![](/assets/img/jingles/dtraces01.png)

This section lists all the currently known D-Trace scripts.

1. Deletes an existing D-Trace script.
2. Creates a new D-Trace script.

### Create<a id="dtraces-new"></a>
![](/assets/img/jingles/dtraces02.png)

Adding a new script is rather simple, it is a plain D-Trace script however there is an exeptation, since the script runs on multiple servers and needs to be configurable. Varlables are encased in `$` signes, and can be choosen freely exept one special placeholder: `$filter$` is the filter created to match the system it is supposed to run on.

<p class="bs-callout bs-callout-info">
Right now FiFo only supports running scripts that output `lquantize`.
</p>

1. The code of the script, with the variables.
2. The name the script is shown as.
3. The variables, they get automatically filled as you type them.
4. Allows for adding variables ahead of time.
5. Click to create.

### Details<a id="dtraces-details"></a>
![](/assets/img/jingles/dtraces03.png)

The details view lets you see the script in it's raw form and the default for valiables it has. However it can not be changed.

![](/assets/img/jingles/dtraces04.png)
This section allows you to run a dtraces script either against individual VM's or Servers. If a VM is selected there is no need to select the servers they are on, FiFo will calculate that automatically.

1. The Server to run upon.
2. The VM's to run against.
3. This section will shouw a heatmap of the output.
4. The variables to run the script with, this can be adjusted to fit the current usecase.
