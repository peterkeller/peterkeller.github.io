---
layout: post
title: Short novice guide for importing a project to Subversion
date: '2012-08-28T17:26:00.001+02:00'
author: Peter Keller
tags:
- Version Control System
modified_time: '2014-06-13T15:49:39.892+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-3822301717428308450
blogger_orig_url: http://peter-on-java.blogspot.com/2012/08/short-novice-guide-for-importing.html
---

My new employer uses <a href="http://subversion.apache.org/">Subversion</a> as the central 
version control system, so I\'m trying to take the first steps as my former employer 
still used the good old CVS. (This also means that you don\'t find advanced infos 
here but only novice infos.)

So, how to import an existing project to Subversion? I don\'t mind loosing the 
history of the project, of course this makes the whole migration process a lot easier.

## 1. Create a Subversion repository

First, a central Subversion repository has to be created if it does not exist already:

{% highlight sh %} 
$ svnadmin create /svn/repos
{% endhighlight %} 

## 2. Import an existing project to the trunk

So, let\'s import the project directory `myproject` to Subversion. I already 
learned that the recommended structure should be as follows:

{% highlight sh %} 
myproject/
 trunk/
 tags/
 branches/
{% endhighlight %}

As in a non-Subversion project, the `trunk`, `tags` and `branches` directories 
usually do not exist yet, they have to be created first. The import sequence for the project directory myproject becomes as follows:

{% highlight sh %} 
$ cd <parent directory of the project directory myproject>
$ mkdir myproject_svn myproject_svn/tags myproject_svn/branches
$ mv myproject myproject_svn/trunk
$ svn import myproject_svn file:///svn/repos/myproject
{% endhighlight %}

As the Subversion repository is locally visible, I use the `file://` schema.

Don\'t forget to add `myproject` to the last command or all files in `svn_myproject` 
will be imported directly into the root directory of the repository.

You cannot work directly with the created `myproject_svn` directory above, 
you first have to checkout it again:

{% highlight sh %} 
$ svn checkout file:///svn/repos/myproject/trunk -d myproject
{% endhighlight %}

## 3. Create a branch

Creating a branch is just a Subversion copy:

{% highlight sh %} 
$ cd myproject
$ svn copy trunk/ branches/myproject-branch
$ svn status
A + branches/myproject-branch
{% endhighlight %}

Here, the `trunk/` directory is copied recursively in the working 
directory `brances/myproject-branch`. The output of the command `svn status` 
indicates that the directory is ready to be added to the repository. 
The `+` says that `branches/myproject-branch` is just a copy and nothing new.

When commiting, Subversion creates the directory in the repository:

{% highlight sh %} 
$ svn commit -m"creating new branch"
Adding     branches/myproject-branch
Committed revision 987.
{% endhighlight %}

Subversion does not really copy all files, but just creates a new repository 
entry pointing to the original tree, so this is also called a "cheap copy". 
Please refer to the Subversion documentation for more detailed infos.

The one step method without the need of a temporary directory is as follows:

{% highlight sh %} 
$ svn copy file:///svn/repos/myproject-trunk \
 file:///svn/repos/myproject/branches/myproject-branch \
 -m"creating new branch"
{% endhighlight %}

