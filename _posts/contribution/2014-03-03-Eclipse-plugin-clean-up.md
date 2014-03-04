---
layout: post
category : contribution
title: Removing old plugins from Eclipse
tagline: "Clean up your Eclipse"
tags : [Eclipse, plugins, clean up, tool]
---
{% include JB/setup %}
---

Removing old plugins is always a hassle. Especially when you upgrade and your Eclipse just doesn't want to start again and you need to figure it out via the metadata hell of plugins dependencies.

Eclipse mechanism tries to address this issue, but it is still not enough. Especially it is really hard to open a uninstall dialog when your Eclipse somehow doens't want to start, right?!
I've seen that couple of you tried to fix this via a script, but it was just a quick work around than a real solution.

### [Eclipse Plugin Cleaner](https://github.com/azachar/eclipse-plugin-cleaner)
It is a brand new **java based tool** (**with tests, build by Maven & hosted at GitHub**) that checks for plugins and features duplicities by introspecting manifest files (or file names if the manifest is incomplete or corrupted). 

Now you can quite easily for example download the latest Eclipse and put your old Eclipse into ``dropins/eclipse`` folder and the tool will clean up the dropins folder - thus your new bundles are preserved and old one are deleted. The ``dropins`` folder is prefered for deletion if 2 or more same versions are found.
So even your old Eclipse did not start, your new Eclipse with the correct configuration will just pick some additional extensions from your old Eclipse and your old installation is recovered and upgraded!

Visit [Eclipse Plugin Cleaner Home Page](https://github.com/azachar/eclipse-plugin-cleaner) for more information.

