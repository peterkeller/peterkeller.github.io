---
layout: post
title: Init Git repository on my Google Drive
date: '2014-06-13T15:45:00.001+02:00'
author: Peter Keller
tags:
- vcs
- macos
modified_time: '2014-06-13T16:07:13.198+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-4658496183554143337
blogger_orig_url: http://peter-on-java.blogspot.com/2014/06/init-git-repository-on-my-google-drive.html
---

Some GIT basics mainly for me to remember. I use Google Drive as a poor man solution to 
share some Java test projects between my work place and my home. And that\'s how I have done it.   

First, init the (local) repository at Google Drive. On MacOS, the directory can be found 
at `~/Google\ Drive/`:

{% highlight sh %}
cd ~/Google\ Drive/
mkdir projects
cd projects
mkdir my_project.git
cd my_project.git
git init --bare
{% endhighlight %}

In the test project workspace, a project is added to the repository as follows:

{% highlight sh %}
cd my_project
git init
git add *
git commit -m "My initial commit message"
git remote add origin ~/Google\ Drive/projects/my_project.git
git push -u origin master
{% endhighlight %}

Now the project can be cloned:

{% highlight sh %}
git clone ~/Google\ Drive/projects/my_project.git
cd my_project
{% endhighlight %}

That\'s it.
