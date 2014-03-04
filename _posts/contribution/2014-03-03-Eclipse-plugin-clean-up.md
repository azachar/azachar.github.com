---
layout: post
category : contribution
title: How to remove old plugins from Eclipse
tagline: "Clean up your Eclipse"
tags : [Eclipse, plugins, clean up, tool]
---
{% include JB/setup %}
---

Removing old plugins is always a hassle. Especially when you upgrade and your Eclipse just doesn't want to start again and you need to figure it out via the metadata hell of plugins dependencies.

I've seen that couple of you tried to fix this via a script. 

Eclipse Plugin Cleaner
======================
Well I have created a **java based tool** (**with tests, build by maven & hosted at github, so you are free to fork it**) that checks for duplicities of plugins by introspecting manifest files (or file names if the manifest is incomplete or corrupted). 

#Now 
you can quite easily for example download the latest Eclipse and put your old Eclipse into ``dropins/eclipse`` folder and the tool will clean up the dropins folder - thus your new bundles are preserved and old one are deleted (The ``dropins`` folder is prefered for deletion if 2 or more same versions are found).

More information about the **Eclipse Plugin Cleaner** can be found at https://github.com/azachar/eclipse-plugin-cleaner

