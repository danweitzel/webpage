---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "GlobalProtect VPN at Colorado State University"
subtitle: ""
summary: ""
authors: [Daniel Weitzel]
tags: []
categories: [software]
date: 2022-10-26T14:10:57+01:00
lastmod: 2022-10-26T14:10:57+01:00
featured: true
draft: false



# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---


In order to be able to access CSU resources off campus or ssh into my work desktop I need to install the GlobalProtect VPN. The installation of the VPN client is pretty straightforward. In this post I'm keeping notes on how to install the CLI version of the client. 

First things first, we need to download the university's VPN package [here](https://col.st/KwjN5). 

This should download a file called `PanGPLinux-6.0.0-c18.tgz`. We need to unpack this file. In order to do that I generated a `pkgs` folder and moved the `.tgz` file.

We can unpack it from the command line with the following command.

```
tar -xvf ~/pkgs/PanGPLinux-6.0.0.tgz
```

The result of this should be a series of `.deb`, `.rpm`, and `.gz` files in the pkgs folder. If you are using an Ubuntu-based operating system you can now install the VPN client with this command:

```
sudo dpkg -i <gp-app-pkg>
```

In my case this was:

```
sudo dpkg -i GlobalProtect_deb-6.0.0.1-44.deb
```

After the installation is complete you can connect to the CSU network with:

```
globalprotect connect --portal gateway.colostate.edu 
```

If you get the following error:

```
Failed to connect to gateway.colostate.edu.
Error: Default browser is not enabled.
```

you need to stop the client:

```
sudo systemctl stop gpd.service
```

add the code below to your `/opt/paloaltonetworks/globalprotect/pangps.xml`

```
<Settings>
<disable-globalprotect>0</disable-globalprotect>
<regioncode>10.0.0.0-10.255.255.255</regioncode>
<default-browser>yes</default-browser>
</Settings>
```

after saving reboot the system.

