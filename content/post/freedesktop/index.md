---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Error message org.freedesktop.Platform 20.08"
subtitle: ""
summary: ""
authors: [Daniel Weitzel]
tags: []
categories: [software]
date: 2023-01-19T09:44:57+01:00
lastmod: 2023-01-19T09:45:00+01:00
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


Updating packages on my Ubuntu based Pop OS operating system recently I got an error message about the end of life of org.freedesktop.Platform 20.08


```
Looking for updates…
Info: org.freedesktop.Platform.GL.default//20.08 is end-of-life, with reason:
   org.freedesktop.Platform 20.08 is no longer receiving fixes and security updates. Please update to a supported runtime version.
```

Solving this is pretty straightforward and can be done in three steps. 

First, check your list of flatpak packages


```
 flatpak list --runtime
```

You should see multiple `Freedesktop SDK` applications with the Application ID `org.freedesktop.Sdk`. You can see the version numbers as well. In my case, there were 20.8, 21.08.17.1, and 22.08.5. 

Using the follwing command we can remove the end of life version:

```
 flatpak remove org.freedesktop.Sdk
```

Running this will give you an option where you can select the version. You should pick `20.08` and afterwards confirm that you want to remove additional folders and files.

You are done with cleaning up. For good measure you can also remove unused packages:

```
 flatpak uninstall --unused
```
